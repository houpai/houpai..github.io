---
title: 解决浏览器自动填充账号input出现背景色的问题
date: 2021-04-28 09:09:55
tags:
- 转载
- Css
- Html
categories:
- 前端
---
记录一下如何解决浏览器自动填充账号input出现背景色的问题...
<!--more-->

##### 解决前:

![image-20210428160033842](https://fastly.jsdelivr.net/gh/houpai/hp-cdn@latest/picGo/image-20210428160033842.png)

##### 解决后：

![image-20210428160115081](https://fastly.jsdelivr.net/gh/houpai/hp-cdn@latest/picGo/image-20210428160115081.png)

##### 解决方法：

- css设置背景色

  ```css
  input:-webkit-autofill { 
      box-shadow: 0 0 0px 1000px white inset !important;
  }
  ```

- input标签添加**autocomplete=“off”** 指定某个文本框取消自动填充

  ```html
  <el-input type="text" v-model="name"  placeholder="请输入账号" autocomplete="off" ></el-input>
  ```

- form表单添加**autocomplete=“off”** 取消所有文本框元素的自动填充

  ```html
  <el-form autocomplete="off">
     ...
  </el-form>
  ```
转载自:[CSDN文章: 解决input输入框自动填充账号密码出现背景色的问题](https://blog.csdn.net/weixin_45899022/article/details/105860397)
  

