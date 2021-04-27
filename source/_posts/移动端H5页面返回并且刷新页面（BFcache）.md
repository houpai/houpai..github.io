---
title: 移动端H5页面返回并且刷新页面（BFcache）
date: 2021-04-27 09:09:55
tags:
- 转载
- javascript
- html
categories:
- 前端
---
什么是BF叉车呢...
<!--more-->

​	项目中的需求：点击浏览器中的返回按钮，要让页面重新加载资源。因为这部分的资源每次去加载的内容都不一样，如果返回的时候，还是看到原先的内容，那做这个内容块的意义就很小了；而如果用户看完了这部分内容，再返回来的时候，这个地方换成了新的内容，这样就能体现这部分的价值了。

​	而对于浏览器来说，大部分浏览器的返回是直接使用缓存的，不会执行任何的javascript代码。原因：部分浏览器在后退时不会触发onload事件，這是HTML5世代浏览器新增的特性之一——Back-Forward Cache(简称bfcache)。

### 什么是bfcache?

​	bfcache，即back-forward cache，可称为“往返缓存”，可以在用户使用浏览器的“后退”和“前进”按钮时加快页面的转换速度。这个缓存不仅保存页面数据，还保存了DOM和JS的状态，实际上是将整个页面都保存在内存里。如果页面位于bfcache中，那么再次打开该页面就不会触发onload事件

### pageshow事件

​	这个事件在用户浏览网页时触发，pageshow 事件类似于 onload 事件，onload 事件在页面第一次加载时触发， pageshow 事件在每次加载页面时触发，即 onload 事件在页面从浏览器缓存中读取时不触发。

### pagehide事件

​	该事件会在用户离开网页时触发。离开网页有多种方式。如点击一个链接，刷新页面，提交表单，关闭浏览器等。pagehide 事件有时可以替代 unload事件，但 unload 事件触发后无法缓存页面。

### persisted属性

​	pageshow事件和pagehide事件的event对象还包含一个名为persisted的布尔值属性。

​	对于pageshow事件，如果页面是从bfcache中加载的，则这个属性的值为true；否则，这个属性的值为false。
​	对于pagehide事件，如果页面在卸载之后被保存在bfcache中，则这个属性的值为true；否则，这个属性的值为false。



​	不同的浏览器在对当前窗口‘打开’历史记录中的前一个页面的表现上并不统一，这和浏览器的实现以及页面本身的设置有关系。

**解决方案：**

**javascript监听pageshow事件阻止页面进入bfcache**

```javascript
 window.addEventListener('pageshow', function (e) {
     if (e.persisted) {
         window.location.reload()
     }
})
```

​	在uc和微信中测试通过，但是在某些安卓手机自带的浏览器中无效。

**javascript监听pagehide事件阻止页面进入bfcache**

```javascript
window.addEventListener('pagehide', function (e) {
    var dom = document.body;
    dom.children.remove();
    setTimeout(function () {
        dom.appendChild("<script type='text/javascript'>window.location.reload();<\/script>");
    });
});
```

**设置meta标签，清除页面缓存**

```html
<meta http-equiv="Cache-Control" content="no-cache, no-store, must-revalidate" />
<meta http-equiv="Pragma" content="no-cache" />
<meta http-equiv="Expires" content="0" />
```

​	Cache-Control指定请求和响应遵循的缓存机制。在请求消息或响应消息中设置Cache-Control并不会修改另一个消息处理过程中的缓存处理过程。请求时的缓存指令包括no-cache、no-store、max-age、max-stale、min-fresh、only-if-cached，响应消息中的指令包括public、private、no-cache、no-store、no-transform、must-revalidate、proxy-revalidate、max-age。各个消息中的指令含义如下
Public指示响应可被任何缓存区缓存
Private指示对于单个用户的整个或部分响应消息，不能被共享缓存处理。这允许服务器仅仅描述当用户的部分响应消息，此响应消息对于其他用户的请求无效
no-cache指示请求或响应消息不能缓存
no-store用于防止重要的信息被无意的发布。在请求消息中发送将使得请求和响应消息都不使用缓存。
max-age指示客户机可以接收生存期不大于指定时间（以秒为单位）的响应
min-fresh指示客户机可以接收响应时间小于当前时间加上指定时间的响应
max-stale指示客户机可以接收超出超时期间的响应消息。如果指定max-stale消息的值，那么客户机可以接收超出超时期指定值之内的响应消息。
注：有些情况下设置清除缓存也没有起到作用，我自己做的这个h5页面就没有起到效果。具体情况还是要具体分析。

**我遇到的情况：**

```javascript
<div class="content">
     <iframe id="iframe" src="https://cpu.baidu.com/xx/xx/xxx" frameborder="no" scrolling="no"></iframe>
</div>
```

​	这个iframe中的地址每次刷新页面都会有不同的内容推送给用户。进入iframe中的内容之后，按返回按钮返回来想进行页面自动刷新，为的就是让用户看到新的内容。

**做法**

​	使用pageshow进行整个页面刷新

```javascript
 window.addEventListener('pageshow', function (e) {
     if (e.persisted) {
         window.location.reload()
     }
 })
```

​	这样可以实现。

​	后面又觉得不妥，没有因为这个小部分而进行整个页面刷新，想到了另一种思路：因为这个iframe中的内容是动态的，可以对其进行定时器设置，如下：

```javascript
 let iframe = document.getElementById('iframe')
 setInterval(() => {
     iframe.setAttribute("src", "https://cpu.baidu.com/xx/xx/xx");
 },15000)
```

​	这样也可以实现自己的功能。

​	最后可以结合一下：

```javascript
let iframe = document.getElementById('iframe')
window.addEventListener('pageshow', function (e) {
    if (e.persisted) {
        iframe.setAttribute("src", "https://cpu.baidu.com/xx/xx/xx");
    }
})
```

​	这样做也有好处，可以避免使用定时器，对网页的性能也是比较好。但是这个方法在返回的时候，可以看到iframe里面内容的重新加载，会有一个时间间隙。


转载自:[CSDN文章: 移动端H5页面返回并且刷新页面（BFcache）](https://www.cnblogs.com/zengfp/p/9910473.html)
