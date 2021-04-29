---
title: termux笔记-解决am命令出现[1] 9205 abort
categories: termux
description: 解决termux安装jdk后am命令出现 [1] 9205 abort
mathjax: true
abbrlink: 1e50c00b
date: 2021-04-25 20:17:46
keywords:
comments:
---
<!--more-->
``` bash
pkg reinstall termux-am
```
重新安装termux-am 包即可

我写了一个小脚本 遍历所有已安装的包进行重新安装
``` bash
#!/bin/env bash
bin=`pkg list-installed | awk '{ print $1}' | sed 's/...........$//g'`
for i in $bin                                                                    do
    echo "正在重新安装$i包" && pkg reinstall $i
done
```
