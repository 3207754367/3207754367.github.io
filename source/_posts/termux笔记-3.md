---
title: termux笔记(3).md
categories: termux
description: 利用magisk模块在termux免root使用adb & fastboot
mathjax: true
tags: 免root使用adb & fadtboot
abbrlink: 2330e9bb
date: 2021-04-27 13:34:37
keywords:
comments:
---
<!--more-->
###  利用 `magisk` 模块 `ADB & Fastboot for Android NDK` 在`termux`中免root使用 `adb & fastboot`

- 克隆magisk模块作者开源的仓库
```
mkdir -p $PREFIX/local/bin && git clone https://github.com/Magisk-Modules-Repo/adb-ndk $PREFIX/local/bin && chmod +x -R $PREFIX/local/bin/adb-ndk/bin
```
- 将克隆下来的仓库加入termux环境变量并使其立即生效
```
echo "export PATH="$PATH:$PREFIX/local/bin/adb-ndk/bin" >> ~/.zshrc && source ~/.zshrc 
```
- 验证是否成功生效(居然支持adb pair 哈哈 )
![验证](https://gitee.com/mpcloud/my_picture_bed/raw/master/6ba0ddab.jpg)

