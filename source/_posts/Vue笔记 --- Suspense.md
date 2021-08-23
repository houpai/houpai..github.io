---
title: Vue3笔记 --- Suspense
date: 2021-08-23 09:09:55
tags:
- Vue3
categories:
- 前端
---
Suspense 组件是 Vue3 中众所周知的特征之一。它可以使我们的 vue 程序在等待异步组件时渲染一些后备内容，从而使用户产生平滑流畅的体验。...

<!--more-->

Suspense 组件很容易理解，也不需要额外导入其他的包。

本文讲告诉你：

- 什么是 Suspense 组件
- 什么时候使用
- 怎样使用

## 什么是 Suspense 组件

Suspense 组件用于在等待某些异步组件来解析时显示后备内容。

使用异步组件的场合非常多，尤其是以下情况：

- 在页面加载之前显示加载动画
- 显示占位符内容
- 处理延迟加载的图像

以前在 vue2 中，我们必须使用条件（`v-if` 或  `v-else`）来检查数据是否已加载和显示后备内容。

但现在 Vue3 有了内置的 Suspense，使我们不必在加载和渲染相应内容时再去额外处理了。

## 怎样使用 Suspense

由于本文的重点是 Suspense 而不是组件 API，所以不会涉及过多关于 API 的细节。

在下面的例子中，ArticleInfo 会在返回之前用异步 `setup` 方法加载用户数据。

```vue
// ArticleInfo.vue
async function getArticleInfo() {
  // 一些异步API调用
  return { article }
}

export default {
  async setup () {
    var { article } = await getArticleInfo()

    return {
      article
    }
  }
}
```

假设 ArticleInfo 被实现在 `partrypost.vue` 中，如果我们想在等待组件获取数据并解析时显示 “Loading Profile...” ，只需三个步骤即可实现 Suspense。

1. 将异步组件包装在 `<template #default>` 标记中
2. 在我们的异步组件旁边添加一个兄弟组件，标签为 `<template #fallback>`
3. 将上述两个组件包装在 `<subsense>` 组件中

通过使用插槽， suspense 将会渲染后备内容，直到默认设置为准备就绪。然后它将自动切换来显示我们的异步组件。

代码如下：

ArticlePost.vue
```vue
<Suspense>
    <template #default>
    <article-info/>
    </template>
    <template #fallback>
    <div>Loading Profile...</div>
    </template>
</Suspense>
```

## 还可以捕获组件错误

vue 有一个不错的功能，是当我们开始使用异步组件时，可以捕获错误并显示给用户一些错误消息。

在 vue2 中，也可以使用 errorCaptured hook实现，但是在 Vue3 中，它被重命名为 `OnErroloIched`。

不管它叫什么名字，这个 hook 都会在捕获到任何子组件的错误时被执行。如果出现问题可以使用 Suspense 渲染错误信息。

下面是代码实现：

ArticlePost.vue
```vue
<template>
  <div v-if="errMsg"> {{ errMsg }} </div>
  <Suspense v-else>
      <template #default>
        <article-info/>
      </template>
      <template #fallback>
        <div>Loading Profile...</div>
      </template>
  </Suspense>
</template>

<script>
import { onErrorCaptured } from 'vue'

setup () {
  const errMsg = ref(null)
  onErrorCaptured(e => {
    errMsg.value = 'Uh oh. Something went wrong!'
    return true
  })}
  return { error }
</script>
```

## 总结

Suspense 只是 Vue 使开我们更容易解决常见问题另一种方式，而不是用来有条件地去渲染组件。

在我看来，它是 Vue3 最巧妙的补充之一





本文章转载自公众号:frontendJS