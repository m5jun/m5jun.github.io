---
layout: post
title: 解决tar解压*.tgz文件出现的 read 150 bytes错误
description: 解决tar解压*.tgz文件出现的 read 150 bytes错误
categories: linux
keywords: linux, tar, 解压
---


### 问题描述
我的开发环境都是通过securCRT远程登录到 linux系统上，所以一般会把本地的*.tgz文件通过rz命令把文件上传到开发机上，上传成功后，我用tar命令解压时，出现以下错误信息：

`tar: read 150 bytes`

### 问题分析
通过rz命令上传文件到开发机，过程中会有一些编码转换的问题，导致最后解压不正常。

### 解决方案
通过samba文件上传到开发机后，再用tar就可以顺利解压并且没有错误出现。或者在用rz上传的时候加-be参数就能解决。



       

 


















