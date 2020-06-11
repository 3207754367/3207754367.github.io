---
title: 在Linux端解包system.new.dat.br
categories: Linux
description: 进来看看在Linux端是如何将system.new.dat.br文件一步一步的变成我们熟悉的system.img的。叭
tags: 教程
abbrlink: 93f66722
date: 2020-06-12 01:51:20
keywords:
comments:
---
<!--more-->
## 把`system.new.dat.br`文件转换成 `system.new.dat`文件
安装`brotli`
```  bash
apt install brotli
```
解压官方卡刷包,进入目录执行命令：
```  bash
brotli -d system.new.dat.br -o system.new.dat
```
等待几分钟即可转换成功

## 把`system.new.dat`文件转换成 `system.img`文件

执行下方的命令获取`sdat2img.py`脚本
``` bash
apt install curl python -y && curl -O  https://raw.githubusercontent.com/xpirt/sdat2img/master/sdat2img.py && chmod +x sdat2img.py
```
运行`sdat2img.py`脚本
``` bash
./sdat2img.py system.transfer.list system.new.dat system.img
```
等待执行成功，脚本将会在当前目录下输出`system.img`文件

## 挂载`system.img`镜像进行查看
创建一个空的文件夹,比如`system`
``` bash
mkdir  system
```
挂载`system.img`镜像至刚刚新增的`system`文件夹
``` bash
mount -o loop system.img  system
```
现在，你可以使用`ls`命令进入`system`文件夹查看镜像内的文件了.

## 将修改后的文件打包回去
获取`make_ext4fs`工具
``` bash
curl -O https://github.com/Loren-Yi/make_ext4fs/raw/master/make_ext4fs && chmod +x make_ext4fs
```
使用 `make_ext4fs`工具打包
```  bash
./make_ext4fs -s  -S file_contexts -l 2048M -a system new_system.img system/
```
命令参数说明：
// -s 表示安静处理，不输出动作，可以不带该参数
// -T 表示Unix时间戳，对system.img中的文件设置修改时间，执行“
date +%s”获取某个时间点的时间戳,也可以直接不用-T 1421464178 
// -S 表示sepolicy 的file_contexts，把该文件放到此目录下，文件取自官方system/root路径或者卡刷包自带（解压内核，在内核里面）
// -l 表示最大的文件大小（受限于分区大小）；可以ls -l 当前转格式出来的system大小、单位也可以为MB
// -a 表示Android的mount点，比如system、userdata、recovery
// new_system.img 表示输出文件名
// system/ 表示输入目录，该目录下有framework、app、bin等目录
