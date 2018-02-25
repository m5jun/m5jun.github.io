---
layout: wiki
title: Linux
categories: Linux
description: linux系统下的一些常用命令和用法
keywords: Linux
---

总结下linux系统下的一些常用命令和用法。

## 实用命令

### fuser

查看文件被谁占用。

```sh
fuser -u .linux.md.swp
```

### id

查看当前用户、组 id。

### lsof

查看打开的文件列表。

> An  open  file  may  be  a  regular  file,  a directory, a block special file, a character special file, an executing text reference, a library, a stream or a network file (Internet socket, NFS file or UNIX domain socket.)  A specific file or all the files in a file system may be selected by path.

#### 查看网络相关的文件占用

```sh
lsof -i
```

#### 查看端口占用

```sh
lsof -i tcp:5037
```

#### 查看某个文件被谁占用

```sh
lsof .linux.md.swp
```

#### 查看某个用户占用的文件信息

```sh
lsof -u mazhuang
```

`-u` 后面可以跟 uid 或 login name。

#### 查看某个程序占用的文件信息

```sh
lsof -c Vim
```

注意程序名区分大小写。

## 查看内存
free命令组合watch用来实时监控内存使用情况:

```
watch -n 2 -d free
```

## 导出堆内存文件

```
jmap -dump:format=b,file=./heap.hprof 17489
```

## 启动httpserver

```
python -m SimpleHTTPServer
```

## 查看java安装的真实位置

```
ls -l `which java`
```

## 域名查找是否生效

```
nslookup xx.baidu.com
````


## 查看目录文件大小

```
du -h --max-depth=1
```

## 高亮匹配的文本

```
cat info.log | grep --color -E "536139"
```

## 监控进程起动

```
supervise -p status/elasticsearch -f "bin/elasticsearch"
```

## 查看TCP统计信息

```
cat /proc/net/netstat
```

## 查看当前系统连接情况

```
cat /proc/net/snmp
```

## 查看网络的统计信息

```
netstat -s
```

## 查看端口使用范围

```
cat /proc/sys/net/ipv4/ip_local_port_range
```

## 查看CPU物理个数

``` 
cat /proc/cpuinfo| grep "physical id"| sort| uniq| wc -l
```

## 查看逻辑CPU的个数

``` 
cat /proc/cpuinfo| grep "processor"| wc -l
```

## 查看每个物理CPU中core的个数(即核数)

总核数 = 物理CPU个数 X 每颗物理CPU的核数 

总逻辑CPU数 = 物理CPU个数 X 每颗物理CPU的核数 X 超线程数

``` 
cat /proc/cpuinfo| grep "cpu cores"| uniq
```

## 在某个目录下查找包含指定字会串的文件

``` 
find .|xargs grep -ri "/v1/finance" -l
```

## 查看磁盘空间

``` 
df -h
```

## 查看文件夹大小

``` 
du --max-depth=1 -h
```

## 查看内存信息

``` 
cat /proc/meminfo 
```

## 查看Linux系统用户最大打开文件限制

``` 
ulimit -n
```



