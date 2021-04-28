---
title: termux笔记
categories: termux
description: 学习termux的笔记 (1)
mathjax: true
tags: termux
abbrlink: d12c2325
date: 2021-04-25 12:50:49
keywords:
comments:
---

<!--more-->
# 使用 Termux 镜像
在较新版的 Termux 中，官方提供了图形界面（TUI）来半自动替换镜像，推荐使用该种方式以规避其他风险。 在 Termux 中执行如下命令
`termux-change-repo`
## 命令行替换
使用如下命令行替换官方源为 TUNA 镜像源
``` bash
sed -i 's@^\(deb.*stable main\)$@#\1\ndeb https://mirrors.tuna.tsinghua.edu.cn/termux/termux-packages-24 stable main@' $PREFIX/etc/apt/sources.list
sed -i 's@^\(deb.*games stable\)$@#\1\ndeb https://mirrors.tuna.tsinghua.edu.cn/termux/game-packages-24 games stable@' $PREFIX/etc/apt/sources.list.d/game.list
sed -i 's@^\(deb.*science stable\)$@#\1\ndeb https://mirrors.tuna.tsinghua.edu.cn/termux/science-packages-24 science stable@' $PREFIX/etc/apt/sources.list.d/science.list
apt update && apt upgrade
```
### 配置openjdk环境
1. 下载 `openjdk`
``` bash
wget https://github.com/Lzhiyong/termux-ndk/releases/download/openjdk/openjdk-11.0.1.tar.xz
```
2. 解压到 `$PREFIX/local` (路径可以自定义 配置环境变量的时候能找到就行)
``` bash
tar -xJvf openjdk-11.0.1.tar.xz -C $PREFIX/local/openjdk-11
```
3. 加入环境变量并使其立即生效（以.zshrc为例）
``` bash
echo 'export JAVA_HOME="$PREFIX/local/openjdk-11"\nexport PATH="$PATH:$JAVA_HOME/bin"' >> ~/.zshrc  && source ~/.zshrc
```
4. 测试是否成功
```
java -version
```
显示java版本信息便是配置成功

![显示java版本信息便是配置成功](https://gitee.com/mpcloud/my_picture_bed/raw/master/ec098c0b3fd24df6c8e16ac1956fbcc1960cbc9e.jpg)

