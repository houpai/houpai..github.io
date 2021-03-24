---
title: 开发工具-git设置代理模式
date: 2021-03-18 10:09:55
tags:
- Git
categories:
- 开发工具

---
github不使用代理就可以访问, 但有的时候clone和push的速度很慢，我们可以使用git代理来提高速度...
<!--more-->

## 设置代理：

### 全局代理(http和https代理)

```code
git config --global http.proxy 127.0.0.1:19180
```

```code
git config --global https.proxy 127.0.0.1:19180
```

### 局部代理(http和https代理)

局部代理，在git仓库内执行

```code
git config --local http.proxy 127.0.0.1:19180
```

```code
git config --local https.proxy 127.0.0.1:19180
```

## 取消代理：

### 取消全局代理(http和https代理)

```code
git config --global --unset http.proxy
```

```code
git config --local --unset http.proxy
```

### 取消局部代理(http和https代理)

```code
git config --local --unset http.proxy
```

```code
git config --local --unset https.proxy
```

------

参考文档:[简述:git设置代理模式，仅为github设置代理](https://www.jianshu.com/p/8c5bb8eee8b2)
