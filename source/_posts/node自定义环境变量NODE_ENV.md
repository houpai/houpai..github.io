---
title: node自定义环境变量NODE_ENV
date: 2021-04-26 10:09:55
tags:
- 原创
- Node
categories:
- 前端
  
---
记录一下如何指定node自定义环境变量NODE_ENV...
<!--more-->

  在很多前端项目中都需要配置node的环境变量，通常在package.json的scripts命令内容和webpack配置文件中可以看到NODE_ENV这个变量，值一般为production或者product，也有人简写为'dev'或'prod'。

#### NODE_ENV的作用

​	 通常这个变量用来区分开发与生产环境，加载不同的配置。

#### 配置

​	node中有全局变量process表示当前node进程，process.env包含着关于系统环境的信息。但是process.env中并不存在NODE_ENV这个东西，NODE_ENV只是一个用户自定义的变量，当我们在服务启动时配置NODE_ENV,或在代码中给process.env.NODE_ENV赋值，js便能通过process.env.NODE_ENV获取信息，通常可以在package.json的scripts里设置

```bash
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "start": "SET NODE_ENV=production && node index.js"
  },
```

一般也可以在webpack配置文件中对NODE_ENV作默认处理

```bash
NODE_ENV: process.env.NODE_ENV || 'development'
```

#### 不同平台下的设置

在类unix系统和安装并使用了bash的windows的系统上:

```bash
"EXPORT  NODE_ENV=production && webpack --config build/webpack.config.js"
```

在windows系统上：

```bash
"SET NODE_ENV=production && webpack --config build/webpack.config.js"
```

还可以引用第三方插件[cross-env](https://www.npmjs.com/package/cross-env)，兼容win和linux

```bash
  "scripts": {
        "start": "cross-env NODE_ENV=production  node index.js"
  }
```
