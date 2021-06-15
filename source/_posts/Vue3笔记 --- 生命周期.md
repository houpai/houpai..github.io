---
title: Vue3笔记 --- 生命周期
date: 2021-06-09 10:09:55
tags:
- Vue3
categories:
- 前端
---
老壶新酒,Vue3 --- 生命周期...
<!--more-->

### 生命周期

这属于老壶新酒了，详情见文档[生命周期钩子函数](https://v3.cn.vuejs.org/api/options-lifecycle-hooks.html)

vue3的生命周期钩子注册函数只能在 setup() 期间同步使用。

因为它们依赖于内部的全局状态来定位当前组件实例（正在调用 setup() 的组件实例）, 不在当前组件下调用这些函数会抛出一个错误。

#### 与 2.x 版本生命周期等效的组合式 API

| **v2.x**      | **v3**          |
| ------------- | --------------- |
| beforeCreate  | **setup**       |
| created       | **setup**       |
| beforeMount   | onBeforeMount   |
| mounted       | onMounted       |
| beforeUpdate  | onBeforeUpdate  |
| updated       | onUpdated       |
| beforeDestroy | onBeforeUnmount |
| destroyed     | onUnmounted     |
| errorCaptured | onErrorCaptured |

#### 新增的钩子函数

| **onRenderTracked**   | 检查哪个 reactive 对象属性或一个 ref 作为依赖被追踪。当render函数被调用时，会检查哪个响应式数据被收集依赖。 |
| --------------------- | ------------------------------------------------------------ |
| **onRenderTriggered** | 当执行update操作时，会检查哪个响应式数据导致组件重新渲染。   |



使用方式与watchEffect的onTrack 和 onTrigger一样：

https://vue-composition-api-rfc.netlify.app/zh/api.html#watcheffect



### Demo

#### App.vue

```vuejs
<template>
  <div id="app">
    <button @click="show=!show">Toggle Demo</button>
    <Demo v-if="show" />
  </div>
</template>

<script>
import Demo from "./components/07Lifecycle.vue";
export default {
  name: "App",
  data() {
    return { show: false };
  },
  components: {
    // HelloWorld,
    Demo,
  },
  errorCaptured(err, vm, info) {
    console.error(err, vm.data, info);
    return true;
  },
};
</script>
```

#### Lifecycle.vue

```vuejs
<template>
  <div>
    <h1>lifecycle - 生命周期</h1>
    <p>{{data.name}}</p>
    <button @click="updateData">Update data</button>
  </div>
</template>

<script>
import {
  reactive,
  // mount
  onBeforeMount,
  onMounted,
  // update
  onBeforeUpdate,
  onUpdated,
  // unmount
  onBeforeUnmount,
  onUnmounted,
  // 新增的钩子函数
  onRenderTracked,
  onRenderTriggered,
} from "vue";

export default {
  name: "Lifecycle",
  setup() {
    onBeforeMount(() => {
      debugger; // eslint-disable-line
    });
    onMounted(() => {
      debugger; // eslint-disable-line
    });
    onBeforeUpdate(() => {
      debugger; // eslint-disable-line
    });
    onUpdated(() => {
      debugger; // eslint-disable-line
    });
    onBeforeUnmount(() => {
      debugger; // eslint-disable-line
    });
    onUnmounted(() => {
      debugger; // eslint-disable-line
    });
    onRenderTracked((e) => { // eslint-disable-line
      debugger; // eslint-disable-line
    });
    onRenderTriggered((e) => { // eslint-disable-line
      debugger; // eslint-disable-line
    });

    const data = reactive({ name: "haihong" });
    const updateData = () => {
      data.name = "chen haiong" + new Date().toLocaleTimeString();
    };
    return { data, updateData };
  },
};
</script>
```

