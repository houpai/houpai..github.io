---
title: 一款vue虚拟键盘(支持中文和手写)插件
date: 2021-05-07 09:09:55
tags:
- vue
- npm
categories:
- 前端
---
推荐一款vue虚拟键盘(支持中文和手写)插件...
<!--more-->



## 关于

### 特性 🎉

- 支持多达五种键盘模式
- 支持自定义主题色
- 已集成丰富的中文字库
- 支持急速识别的手写能力
- vue3.0 组件开箱即用

## 支持环境

| [![IE / Edge](https://raw.githubusercontent.com/alrra/browser-logos/master/src/edge/edge_48x48.png)](http://godban.github.io/browsers-support-badges/) Edge | [![Firefox](https://raw.githubusercontent.com/alrra/browser-logos/master/src/firefox/firefox_48x48.png)](http://godban.github.io/browsers-support-badges/)Firefox | [![Chrome](https://raw.githubusercontent.com/alrra/browser-logos/master/src/chrome/chrome_48x48.png)](http://godban.github.io/browsers-support-badges/)Chrome | [![Safari](https://raw.githubusercontent.com/alrra/browser-logos/master/src/safari/safari_48x48.png)](http://godban.github.io/browsers-support-badges/)Safari | [![Opera](https://raw.githubusercontent.com/alrra/browser-logos/master/src/opera/opera_48x48.png)](http://godban.github.io/browsers-support-badges/)Opera |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Edge                                                         | last 2 versions                                              | last 2 versions                                              | last 2 versions                                              | last 2 versions                                              |

## 安装

### 使用 npm 或 yarn 安装

```
$ npm install vue-keyboard-virtual-next --save
$ yarn add vue-keyboard-virtual-next --save
```

如果你的网络环境不佳，推荐使用 [cnpm](https://github.com/cnpm/cnpm)。

## 使用

### 全局引入

```javascript
import App from "./app.vue";
import { createApp } from "vue";
import "vue-keyboard-virtual-next/keyboard.min.css";
import KeyBoard from "vue-keyboard-virtual-next";

createApp(App)
  .use(keyBoard)
  .mount("#app");
```

### 局部引入

```html
<template>
  <!-- keyboard 只识别带有 data-mode 标识的输入框 -->
  <input data-mode v-model="value" />
  <Key-Board 
  	:handApi="'https://service.chaunve.com/HandWriteRecognizerService.asmx/Command'" 
  	:color="'#06f'"
  />
</template>
```
```javascript
<script lang="ts">
  import "keyboard-virtual-vue/keyboard.min.css";
  import KeyBoard from "keyboard-virtual-vue";
  import { defineComponent, ref } from "vue";
  export default defineComponent({
    components: { KeyBoard },
    setup() {
      const value = ref<string>("你好");
      return {
        value,
      };
    },
  });
</script>
```

### 配置参数

### Input标签属性

| 属性          | 说明                                                         | 类型   | 可选值                             | 默认值         |
| ------------- | ------------------------------------------------------------ | ------ | ---------------------------------- | -------------- |
| **data-mode** | 弹出输入法的类型： `en` 英文小写 `number`数字 `symbol` 标点 `handwrite` 手写 `不传` 默认中文 | String | `en` `number` `symbol` `handwrite` | `default as *` |
| **data-prop** | 注册的输入框的类型                                           | String | *                                  |                |

### Props属性

| 参数              | 说明                                                         | 默认值                  | 类型           | 是否必须 | 版本    |
| ----------------- | ------------------------------------------------------------ | ----------------------- | -------------- | -------- | ------- |
| v-model           | *绑定的输入框value*,可同时双向绑定多个输入框，不传则只与当前focus输入框做数据绑定关系 |                         | string\|number | 否       | v1.0.0+ |
| color             | *主题色*                                                     | `#eaa050`               | string         | 否       | v1.0.0+ |
| modeList          | *键盘渲染模式列表*，若不传handApi则不会出现手写板            | ["handwrite", "symbol"] | string[]       | 否       | v1.0.0+ |
| blurHide          | *是否当前输入框blur事件触发隐藏*                             | true                    | boolean        | 否       | v1.0.0+ |
| showHandleBar     | *是否显示拖拽句柄*                                           | true                    | boolean        | 否       | v1.0.0+ |
| dargHandleText    | 拖拽句柄提示文字                                             |                         | string         | 否       | v1.0.0+ |
| modal             | *是否显示遮罩层*                                             | false                   | boolean        | 否       | v1.0.0+ |
| closeOnClickModal | 是否点击遮罩层隐藏键盘                                       | true                    | boolean        | 否       | v1.0.0+ |
| handApi           | 手写识别接口，若不传则无法切换手写模块                       |                         | string         | 否       | v1.0.0+ |
| animateClass      | 键盘显隐动画，内置slide动画，如若需要其他动画，可传入相应类名自定义动画 |                         | string         | 否       | v1.0.0+ |

### Events

| 参数       | 说明                                                         | 类型                                                    | 版本    |
| ---------- | ------------------------------------------------------------ | ------------------------------------------------------- | ------- |
| keyChange  | 按键触发事件，第一个参数为当前触发的按键的标识,`第二个参数为当前聚焦输入框的props值，若没有则直接返回当前聚焦的input元素（v1.0.1版本之后）` | (*value*: string,prop:string\|HTMLInputElement) => void | v1.0.0+ |
| change     | value改变事件，第一个参数为当前最新通过键盘输入的值，`第二个参数为当前聚焦输入框的props值，若没有则直接返回当前聚焦的input元素（v1.0.1版本之后）` | (*value*: string,prop:string\|HTMLInputElement) => void | v1.0.0+ |
| closed     | 键盘关闭事件                                                 | () => void                                              | v1.0.0+ |
| modalClick | 遮罩点击事件                                                 | () => void                                              | v1.0.0+ |

## Component Event

| 方法名          | 说明                                                         | 类型                            | 版本    |
| --------------- | ------------------------------------------------------------ | ------------------------------- | ------- |
| reSignUp        | 重新给input注册绑定键盘,当页面有新的input标签出现时调用此方法 | （）=> void                     | v1.0.0+ |
| getCurrentInput | 获取当前聚焦的输入框                                         | （）=> HTMLInputElement \| null | v1.0.1+ |

```vue
<template>
  <!-- keyboard 只识别带有 data-mode 标识的输入框 -->
  <input data-mode v-model="value" />
  <Key-Board 
  	:handApi="'https://service.chaunve.com/HandWriteRecognizerService.asmx/Command'" 
  	:color="'#06f'"
  />
</template>

<script lang="ts">
import "vue-keyboard-virtual-next/keyboard.min.css";
import KeyBoard from "vue-keyboard-virtual-next";
import { defineComponent, ref, onMounted } from "vue";
export default defineComponent({
  components: { KeyBoard },
  setup() {
    const value = ref<string>("你好");
    const keyBoardRef = ref<typeof KeyBoard | null>(null);
      
    onMounted(() => {
        // xxxx逻辑 给键盘重新注册输入框
        keyBoardRef.value?.reSignUp();
    })
      
    return {
      value,
      keyBoardRef
    };
  },
});
</script>
```
