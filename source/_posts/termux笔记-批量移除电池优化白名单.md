---
title: termux笔记-批量移除电池优化白名单
categories: termux
comments: true
description: 利用shizuku免root批量移除电池优化白名单
mathjax: true
abbrlink: dbe891cd
thumbnail:
---
<!--more-->

### 基于shizuku

```  bash
#!/bin/env bash

shizuku dumpsys deviceidle whitelist | grep user | sed 's/user,//g' | sed 's/[0-9]//g' | sed 's/,//g' | sed 's/^/shizuku dumpsys deviceidle whitelist -/g' |sh
#所有user级别应用移出优化白名单
#将QQ和微信重新加入优化白名单
for i in com.tencent.mobileqq com.tencent.mm
do
    shizuku dumpsys deviceidle whitelist +$i
    echo "$i已重新加入优化白名单"
done

```
