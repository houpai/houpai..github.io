---
title: 在微信小程序中使用 Protocol Buffer 的方法(如无必要,请慎用)
date: 2021-10-14 09:09:55
tags:
- 微信小程序
- 转载
  categories:
- 前端
---
在微信小程序中使用 Protocol Buffer 的方法...
<!--more-->

## 前言

我去年写过一套自用的微服务网关系统，投入线上使用后效果良好，响应格式采用的是使用广泛的 JSON。最近为了使其支持二进制数据的传输，考虑到对二进制数据进行 base64 等格式的编码同时降低了时空性能，一番简单的搜索后，决定采用 Google 推出的 Protocol Buffer(后文简称 protobuf) 作为消息载体格式替换掉 JSON。

在服务端部分完全升级至 protobuf 以后，微信小程序客户端方面的支持却让我恼火了很久，因为官方对于 protobuf 的支持是无动于衷的。在这之前，我本以为在微信小程序端发起请求就像前端一样简单容易。

## 尝试

protobuf 是强约定的消息格式，消息必须预先编写语言无关的 proto 文件进行消息定义，然后通过 protoc 编译成目标语言的模型文件。

Google 官方提供的 Javascript 的支持无法在微信小程序平台使用，因为出于安全考虑，微信禁止了诸如动态执行 eval, function 等功能。

社区实现的 protobufjs 的最新版**原版似乎**也不能够在微信小程序平台使用，原因同上。未深入研究。

最终找到的可行的方案：https://github.com/Zhang19910325/protoBufferForWechat，是由国人对 protobufjs 项目进行修改后的微信适配版，虽然较长时间得不到维护，但依然可用。

## 准备工作

与 Google 的方案一样，protobufjs 也需要我们对 proto 文件进行解析，根据网络上的信息，我们需要首先安装 protobufjs 工具来将 proto 文件转制成 json 格式文件。

```
npm install -g protobufjs
pbjs -t json my.proto > my.json
```

为了方便项目导入，我们需要将 json 文件包装成 js 文件。将 json 文件用模块导出进行包裹：

```
module.exports = { /* some stuff */ };
```

准备好后，可以自行写一个包处理模块，来处理收发包：

```javascript
'use strict';

const protobuf = require('./weichatPb/protobuf.js');
const requestPacketDefinition = require('./RequestPacket.js');
const responsePacketDefinition = require('./ResponsePacket.js');
const RequestPacket = protobuf.Root.fromJSON(requestPacketDefinition).Collplex.Models.RequestPacket;
const ResponsePacket = protobuf.Root.fromJSON(responsePacketDefinition).Collplex.Models.ResponsePacket;

module.exports = {
  makeRequestPacket: (payload, clientId, key) => (RequestPacket.lookup('RequestPacket').encode({
    clientId,
    action: RequestPacket.ActionType.SVC_REQUEST,
    data: RequestPacket.lookup('ServiceRequest').encode({
      key,
      data: JSON.stringify(payload),
    }).finish()
  }).finish()).slice().buffer,
  decodeResponsePacket: data => ResponsePacket.lookup('ResponsePacket').decode(data),
};
```

## 填坑

```javascript
wx.request({
      url: that.apiUrlPrefix,
      data: payload,
      method: 'POST',
      header: {
        'Content-Type': 'application/x-protobuf'
      },
      dataType: 'other',
      responseType: 'arraybuffer',
});
```

虽然我们在模块里包装好了封包解包的方法，但是还是不能直接在微信小程序中发送的。让我们先来看看 wx.request() 的[官方文档](https://developers.weixin.qq.com/miniprogram/dev/api/network/request/wx.request.html)：

> ### data 参数说明
>
> 最终发送给服务器的数据是 String 类型，**如果传入的 data 不是 String 类型，会被转换成 String** 。转换规则如下：
>
> - 对于 `GET` 方法的数据，会将数据转换成 query string（`encodeURIComponent(k)=encodeURIComponent(v)&encodeURIComponent(k)=encodeURIComponent(v)...`）
> - 对于 `POST` 方法且 `header['content-type']` 为 `application/json` 的数据，会对数据进行 JSON 序列化
> - 对于 `POST` 方法且 `header['content-type']` 为 `application/x-www-form-urlencoded` 的数据，会将数据转换成 query string `（encodeURIComponent(k)=encodeURIComponent(v)&encodeURIComponent(k)=encodeURIComponent(v)...）`

实际测试下来，data 支持三种数据类型：String / Object（会被序列化或转换成 query string）/ ArrayBuffer。因为我们的 protobuf 在二进制序列化后，给出的类型是 Uint8Array，微信小程序会按照 Object 进行编码，导致服务端接收到异常的数据，所以我们应该将 Uint8Array 转换成 ArrayBuffer。

```javascript
Uint8Array.slice().buffer
```

我们可以注意到，这里使用了 slice()。这是因为 protobufjs 转换出来的 ArrayBuffer 的缓冲区长度是 8912 字节，如果不进行处理，则会发送至少 8192 个字节给服务端。

准备好发送的数据后，我们就可以收到服务端发送的正确响应了。

如果你的 proto 中包含了以二进制格式存储的字符串，那么需要进行解码提取操作。在模拟器上，可以使用：

```
new TextDecoder().decode(decodedResponsePacket);
```

但是，**在真机上不可以这么做**！因为微信把 TextEncoder 与 TextDecoder 给阉割掉了。我们必须寻找一种替代方案，来手动实现转码：

```javascript
utf8ArrayToStr: array => {
    let out, i, len, c;
    let char2, char3;
    out = "";
    len = array.length;
    i = 0;
    while(i < len) {
      c = array[i++];
      switch(c >> 4)
      { 
          case 0: case 1: case 2: case 3: case 4: case 5: case 6: case 7:
          // 0xxxxxxx
          out += String.fromCharCode(c);
          break;
          case 12: case 13:
          // 110x xxxx   10xx xxxx
          char2 = array[i++];
          out += String.fromCharCode(((c & 0x1F) << 6) | (char2 & 0x3F));
          break;
          case 14:
          // 1110 xxxx  10xx xxxx  10xx xxxx
          char2 = array[i++];
          char3 = array[i++];
          out += String.fromCharCode(((c & 0x0F) << 12) |
                        ((char2 & 0x3F) << 6) |
                        ((char3 & 0x3F) << 0));
          break;
        }
      }
      return out;
}
```

以上代码来自网络，性能较低。可以基本正确地将二进制数据转换为 UTF-8 格式的字符串，但会丢失部分 Emoji 字符。

## 总结

如无必要，不要使用非官方支持的格式，太折腾且有风险。本次迁移数据格式也是为了让微服务网关能够兼容后续项目，支持二进制数据而进行的。

最后，新版本微服务框架工作情况尚可，可喜可贺~

![](https://cdn.jsdelivr.net/gh/houpai/hp-cdn@latest/picGo/CollplexDemo-1024x494.png)

转载于:[在微信小程序中使用 Protocol Buffer 的方法](https://iedon.com/2020/08/18/790.html)

