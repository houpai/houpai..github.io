---
title: github配置ssh
date: 2021-04-08 10:09:55
tags:
- Git
categories:
- 开发工具
---

为什么要配置这个呢？因为你提交代码肯定要拥有你的github权限才可以，但是直接使用用户名和密码太不安全了，所以我们使用ssh key来解决本地和服务器的连接问题。
<!--more-->

#### 1.从程序目录打开Git Bash

#### 2.ssh-keygen -t rsa -C "email@email.com" ,email@email.com是自己github账号

#### 3.提醒你输入key的名称，你可以不用输入，一路回车，就OK了

#### 4.一般会在用户目录下生成三个文件，例如我生成的文件在C:\Users\xxx\\.ssh目录下

#### 5.进入该用户的目录下用命令cat id_rsa.pub打开文件并复制里面的全部内容

#### 6.打开github账号–>Settings–>SSH and GPG keys–>New SSH key,并把复制好的内容全部粘贴进去



![](https://cdn.jsdelivr.net/gh/houpai/hp-cdn@latest/ssh1.png)

![](https://cdn.jsdelivr.net/gh/houpai/hp-cdn@latest/ssh2.png)

![](https://cdn.jsdelivr.net/gh/houpai/hp-cdn@latest/git_ssh.png)
