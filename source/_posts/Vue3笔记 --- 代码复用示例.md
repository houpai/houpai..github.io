---
title: Vue3笔记 --- 代码复用示例
date: 2021-06-15 10:09:55
tags:
- Vue3
categories:
- 前端

---
记录整理Vue3代码的复用示例...
<!--more-->
```vue
<template>
  <div>
    <h1>组合式api架构</h1>
    <h2>usemouse</h2>
    <p>position x: {{x}}</p>
    <p>position y: {{y}}</p>
    <h2>useCount</h2>
    <p>{{count}}</p>
    <button @click="add">Add</button>
    <button @click="minus">Minus</button>
  </div>
</template>

<script>
import { onMounted, onUnmounted, ref } from "vue";
export default {
  name: "CompositionApiArchitecture",
  setup() {
    const { x, y } = useMouse();
    const { count, add, minus } = useCount();
    return { x, y, count, add, minus };
  },
};
  
function useMouse() {
  const x = ref(0);
  const y = ref(0);
  const updateXY = (e) => {
    x.value = e.x;
    y.value = e.y;
  };
  onMounted(() => {
    document.addEventListener("mousemove", updateXY);
  });
  onUnmounted(() => {
    document.removeEventListener("mousemove", updateXY);
  });
  return { x, y };
}
  
function useCount() {
  const count = ref(0);
  const add = () => count.value++;
  const minus = () => count.value--;
  return { count, add, minus };
}
</script>
```



![image-20210615133627881](https://cdn.jsdelivr.net/gh/houpai/hp-cdn@latest/picGo/image-20210615133627881.png)
