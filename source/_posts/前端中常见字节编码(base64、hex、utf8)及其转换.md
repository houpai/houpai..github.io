---
title: 前端中常见字节编码(base64、hex、utf8)及其转换
date: 2021-05-07 11:09:55
tags:
- Javascript
- 转载
categories:
- 前端
---
前端中常见字节编码(base64、hex、utf8)及其转换...
<!--more-->

本文章向大家介绍前端中常见字节编码(base64、hex、utf8)及其转换，主要包括前端中常见字节编码(base64、hex、utf8)及其转换使用实例、应用技巧、基本知识点总结和需要注意事项，具有一定的参考价值，需要的朋友可以参考一下。

```javascript
/*
* 字节编码转换
* 首先都需要转为二级制数组 (ArrayBuffer)
* 然后才能转换对应的编码字符
* 前端常见编码：
* base64：就是将二进制转为字符串,将每6个字节转为一个特定的字符串(A-Za-z0-9/+=)。
* hex：将二进制每8个字节转为对应的2个十六进制的字符串
* */

// utf8 转为 base64/hex
let output = Buffer.from('utf8的字符串', 'utf8')
console.log(output.toString('base64'))
console.log(output.toString('hex'))


// base64/hex 转为 utf8
output = Buffer.from('75746638e79a84e5ad97e7aca6e4b8b2', 'hex')
console.log(output.toString('utf8'))
output = Buffer.from('dXRmOOeahOWtl+espuS4sg==', 'base64')
console.log(output.toString('utf8'))


// 读取文件传入编码
input = fs.readFileSync('test.txt')  // 默认是二进制 Buffer
console.log(input)
let input = fs.readFileSync('test.txt', 'utf8')
console.log(input)
input = fs.readFileSync('test.txt', 'base64')
console.log(input)
input = fs.readFileSync('test.txt', 'hex')
console.log(input)
```

　　

```
/*
* 加密需注意
* 加密数据类型：Buffer 或者 字符串(hex/base64/utf8)
* 参数传入参数：vi - 填充
* 参数传入参数：mode - 模式
* 参数传入参数：padding - 填充类型
* 加密输出类型：Buffer 或者 字符串(hex/base64/utf8)
* */
```

　　

原文地址：https://www.cnblogs.com/jiebba/p/12023652.html
