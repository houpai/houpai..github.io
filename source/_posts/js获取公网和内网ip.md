---
title: js获取公网和内网ip
date: 2021-05-27 10:09:55
tags:
- Javascript
- 转载
categories:
- 前端

---
记录一下js获取公网和内网ip的方法
<!--more-->


貌似至少一年前就被提出来过的了，就算用 SS 和 VPN 也无法避免 —— 只要你用的是 Chrome 或者 Firefox 浏览器。

之前看到过有关讨论，但是因为不会 javascript 就没关注，这个寒假补了下 javascript 和 jQuery ，倒是可以看懂一些了。

Demo: https://diafygi.github.io/webrtc-ips/

> What this does
>
> 
>
> Firefox and Chrome have implemented WebRTC that allow requests to STUN
> servers be made that will return the local and public IP addresses for
> the user. These request results are available to javascript, so you
> can now obtain a users local and public IP addresses in javascript.
> This demo is an example implementation of that.
>
> Additionally, these STUN requests are made outside of the normal
> XMLHttpRequest procedure, so they are not visible in the developer
> console or able to be blocked by plugins such as AdBlockPlus or
> Ghostery. This makes these types of requests available for online
> tracking if an advertiser sets up a STUN server with a wildcard
> domain.

就是说 Firefox 和 Chrome 内建的 WebRTC 允许向 STUN 服务器查询用户的本地和公共 IP 地址。查询结果对 javascript 可用，所以可以使用 javascript 得到用户的本地和公网地址。

<font color=red>但是需要特别注意的是需要打开浏览器的响应配置才可以获取到，操作见下图</font>

![image-20210527174417119](https://cdn.jsdelivr.net/gh/houpai/hp-cdn@latest/picGo/image-20210527174417119.png)

复制粘贴一下代码：

```html
<!DOCTYPE html>
<html>
<head>
  <meta http-equiv="Content-Type" content="text/html; charset=utf-8">
</head>
<body>
<h4>
  Demo for:
  <a href="https://github.com/diafygi/webrtc-ips">
    https://github.com/diafygi/webrtc-ips
  </a>
</h4>
<p>
  This demo secretly makes requests to STUN servers that can log your
  request. These requests do not show up in developer consoles and
  cannot be blocked by browser plugins (AdBlock, Ghostery, etc.).
</p>
<h4>Your local IP addresses:</h4>
<ul></ul>
<h4>Your public IP addresses:</h4>
<ul></ul>
<script>
  //get the IP addresses associated with an account
  function getIPs(callback){
    var ip_dups = {};
    //compatibility for firefox and chrome
    var RTCPeerConnection = window.RTCPeerConnection
      || window.mozRTCPeerConnection
      || window.webkitRTCPeerConnection;
    var useWebKit = !!window.webkitRTCPeerConnection;
    //bypass naive webrtc blocking
    if(!RTCPeerConnection){
      //create an iframe node
      var iframe = document.createElement('iframe');
      iframe.style.display = 'none';
      //invalidate content script
      iframe.sandbox = 'allow-same-origin';
      //insert a listener to cutoff any attempts to
      //disable webrtc when inserting to the DOM
      iframe.addEventListener("DOMNodeInserted", function(e){
        e.stopPropagation();
      }, false);
      iframe.addEventListener("DOMNodeInsertedIntoDocument", function(e){
        e.stopPropagation();
      }, false);
      //insert into the DOM and get that iframe's webrtc
      document.body.appendChild(iframe);
      var win = iframe.contentWindow;
      RTCPeerConnection = win.RTCPeerConnection
        || win.mozRTCPeerConnection
        || win.webkitRTCPeerConnection;
      useWebKit = !!win.webkitRTCPeerConnection;
    }
    //minimal requirements for data connection
    var mediaConstraints = {
      optional: [{RtpDataChannels: true}]
    };
    //firefox already has a default stun server in about:config
    //    media.peerconnection.default_iceservers =
    //    [{"url": "stun:stun.services.mozilla.com"}]
    var servers = undefined;
    //add same stun server for chrome
    if(useWebKit)
      servers = {iceServers: [{urls: "stun:stun.services.mozilla.com"}]};
    //construct a new RTCPeerConnection
    var pc = new RTCPeerConnection(servers, mediaConstraints);
    function handleCandidate(candidate){
      //match just the IP address
      var ip_regex = /([0-9]{1,3}(\.[0-9]{1,3}){3})/
      var ip_addr = ip_regex.exec(candidate)[1];
      //remove duplicates
      if(ip_dups[ip_addr] === undefined)
        callback(ip_addr);
      ip_dups[ip_addr] = true;
    }
    //listen for candidate events
    pc.onicecandidate = function(ice){
      //skip non-candidate events
      if(ice.candidate)
        handleCandidate(ice.candidate.candidate);
    };
    //create a bogus data channel
    pc.createDataChannel("");
    //create an offer sdp
    pc.createOffer(function(result){
      //trigger the stun server request
      pc.setLocalDescription(result, function(){}, function(){});
    }, function(){});
    //wait for a while to let everything done
    setTimeout(function(){
      //read candidate info from local description
      var lines = pc.localDescription.sdp.split('\n');
      lines.forEach(function(line){
        if(line.indexOf('a=candidate:') === 0)
          handleCandidate(line);
      });
    }, 1000);
  }
  //insert IP addresses into the page
  getIPs(function(ip){
    var li = document.createElement("li");
    li.textContent = ip;
    //local IPs
    if (ip.match(/^(192\.168\.|169\.254\.|10\.|172\.(1[6-9]|2\d|3[01]))/))
      document.getElementsByTagName("ul")[0].appendChild(li);
    //assume the rest are public IPs
    else
      document.getElementsByTagName("ul")[1].appendChild(li);
  });
</script>
</body>
</html>
```

**Wooyun 上一些 js 的其他用法：**

> 获得flash版本 http://jsbin.com/rukirayuca
> 获得java版本 http://jsbin.com/cabirudale
> 获得插件列表 http://jsbin.com/vejujatuxa
> 获得内网IP http://jsbin.com/riyisavura
> 扫描HTTP端口 http://jsbin.com/ziwununivo
> 扫描WEB容器 http://jsbin.com/piwemaquwa
> 扫描FTP端口 http://jsbin.com/kulahicide



转载于:[通过WebRTC获取内网和公网真实IP地址](http://0x0d.im/archives/get-local-and-real-public-IP-via-WebRTC.html)
