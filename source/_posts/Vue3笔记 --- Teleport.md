---
title: Vue3笔记 --- Teleport
date: 2021-08-19 09:09:55
tags:
- Vue3
categories:
- 前端
---
Vue3之新特性Teleport,真香...
<!--more-->



在`vue2.0`时代，我们经常会有这样的需求，写代码逻辑的时候希望将组件写在某个模板之下，因为这样我们很好的使用组件内部的状态数据，控制组件的展示形态。但是从技术的角度上我们又希望将这段代码移到`DOM`中`Vue app`之外的其他位置。

举个简单的例子，我们在使用`modal`组件的时候，我们将它放在了我们的模板`template`里面，但是由于`modal`组件希望位于页面的最上方，这时候我们将`modal`组件挂载在`body`上面是最好控制的，我们能够很好的通过`zIndex`来控制`modal`的位置，当他嵌套在`template`里面的时候就不那么容易了。

#### Vue2中的实现

​	`vue2.0`中我在写这个组件的时候是通过手动的形式来进行挂载的，我写了一个vue指令来进行这个操作，帮助我将`modal`组件挂载到`body`上面去，专这样也能够很好的通过控制`zIndex`来控制`modal`的展示。

```javascript
function insert(el) {
  const parent = el.parentNode;
  if (parent && parent !== document.body) {
      parent.removeChild(el);
      document.body.appendChild(el);
  }
}

export default (typeof window !== 'undefined' ? {
  inserted(el, { value }) {
      if (value) {
          insert(el);
      }
  },
  componentUpdated(el, { value }) {
      if (value) {
          insert(el);
      }
  },
} : {});
```

上面的代码其实就是简单的将`modal`从他原始挂载的父节点移除，然后挂载到`body`上去，通过手动的形式来重新挂载，能够很好的解决这种问题，当然上面只是简单的逻辑，如果需要考虑卸载等其他逻辑代码还得增加。

```javascript
<template>
  <div class="modal" v-to-body="show" v-if="show">
    <div class="modal-mask" @click="close"></div>
    <slot></slot>
  </div>
</template>

<script>
import "./style.scss";
import toBody from "../directives/to-body";
export default {
  props: {
    show: Boolean,
  },
  directives: {
    toBody,
  },
  methods: {
    close() {
      this.$emit("close");
    },
  },
};
</script>
```

说实话`vue2.0`中的实现其实是没啥问题的，只是不是很优雅，需要额外的代码控制，所以`vue3.0`中直接带来了`Teleport-任意传送门`

具体代码参考vue2.0-modal: https://codesandbox.io/s/vue20-modal-sc1rq

#### 什么是Teleport

`Teleport`能够直接帮助我们将组件渲染后页面中的任意地方，只要我们指定了渲染的目标对象。`Teleport`使用起来非常简单。

```javascript
<template>
  <teleport to="body" class="modal" v-if="show">
    <div class="modal-mask" @click="close"></div>
    <slot></slot>
  </teleport>
</template>

<script>
import "./style.scss";
export default {
  props: {
    show: Boolean,
  },
  methods: {
    close() {
      this.$emit("close");
    },
  },
};
</script>
```

上面的代码我们就能够很简单的实现之前vue2.0所实现的功能。

具体代码参考vue3.0-modal: https://codesandbox.io/s/vue3-modal-x2lud

#### **注意点**

##### 与 Vue components 一起使用

在这种情况下，即使在不同的地方渲染`child-component`，它仍将是`parent-component`的子级，并将从中接收`name prop`。

这也意味着来自父组件的注入按预期工作，并且子组件将嵌套在`Vue Devtools`中的父组件之下，而不是放在实际内容移动到的位置。

```javascript
const app = Vue.createApp({
  template: `
    <h1>Root instance</h1>
    <parent-component />
  `
})

app.component('parent-component', {
  template: `
    <h2>This is a parent component</h2>
    <teleport to="#endofbody">
      <child-component name="John" />
    </teleport>
  `
})

app.component('child-component', {
  props: ['name'],
  template: `
    <div>Hello, {{ name }}</div>
  `
})
```

##### 在同一目标上使用多个teleport

当我们将多个`teleport`送到同一位置时会发生什么？

```javascript
<teleport to="#modals">
  <div>A</div>
</teleport>
<teleport to="#modals">
  <div>B</div>
</teleport>

<!-- result-->
<div id="modals">
  <div>A</div>
  <div>B</div>
</div>
```

我们可以看到对于这种情况，多个`teleport`组件可以将其内容挂载到同一个目标元素。顺序将是一个简单的追加——稍后挂载将位于目标元素中较早的挂载之后。

#### 总结

一句话来描述`Teleport`就是一种将代码组织逻辑依旧放在组件中，这样我们能够使用组件内部的数据状态，控制组件展示的形式，但是最后渲染的地方可以是任意的，而不是局限于组件内部

\- END -

点赞 + 在看 + 分享，一键三连，幸福满满！

本文分享自微信公众号 - 鱼头的Web海洋（krissarea）

原文出处及转载信息见文内详细说明，如有侵权，请联系 yunjia_community@tencent.com 删除。

原始发表时间：2021-07-05

本文参与[腾讯云自媒体分享计划](https://cloud.tencent.com/developer/support-plan)，欢迎正在阅读的你也加入，一起分享。

