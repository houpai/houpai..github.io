---
title: 开发工具-git设置代理模式
date: 2021-03-18 10:09:55
tags:
- Git
categories:
- 开发工具

---
为git设置代理,分为全局代理和局部代理
<!--more-->

## 设置代理：

### 全局代理

git config --global http.proxy `[IP地址]`:`[端口号]`

**例:**git config --global http.proxy 127.0.0.1:1087

### 局部代理，在github clone 仓库内执行

git config --local http.proxy `[IP地址]`:`[端口号]`

**例:**git config --local http.proxy 127.0.0.1:1087

## 查询是否使用代理：

### 查询全局代理

git config --global http.proxy

### 查询局部代理

git config --local http.proxy

## 取消代理：

git config --global --unset http.proxy
git config --local --unset http.proxy

------

参考文档:[简述:git设置代理模式，仅为github设置代理](https://www.jianshu.com/p/8c5bb8eee8b2)
