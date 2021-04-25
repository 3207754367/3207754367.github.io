---
title: termux笔记(2).md
categories: termux
description: Termux笔记 (2)
mathjax: true
abbrlink: 1e50c00b
date: 2021-04-25 20:17:46
tags:
keywords:
comments:
---
<!--more-->

## 利用shizuku免root卸载部分系统APP

``` bash
#!/bin/env bash

start_time=`date --date='0 days ago' "+%Y-%m-%d %H:%M:%S"`
pkg="
com.miui.systemAdSolution
com.miui.analytics
com.miui.player
com.mipay.wallet
com.miui.newmidrive
com.miui.newhome
com.miui.userguide
com.miui.translation.youdao
com.miui.miservice
com.xiaomi.gamecenter
com.xiaomi.jr
com.xiaomi.youpin
com.xiaomi.ab
com.android.email
com.mi.liveassistant
com.dragon.read
com.ss.android.article.news
"
echo "
本脚本使用shizuku的权限进行卸载操作
请确认shizuku是否已激活

			———— 2020.3.24
"
echo "正在检查依赖项..."
function run (){
for o in ${pkg}
do
    n=`shizuku pm path $o | sed 's/package://g'`
    for j in $n
    do 
	m=`aapt d badging $j | grep 'application-label-zh-CN:' | sed 's/application-label-zh-CN://g'`
	#m=`aapt d badging $j | grep 'package' | awk '{ print $2 }' | sed 's/name=//g'`
	#m=`aapt d badging $j | grep "application:" | awk '{ print $2}' | sed 's/label=//g'`
	shizuku pm uninstall --user 0 $o > /dev/null
	for c in $m
	do
	    echo "应用 $c 已从机主账户上卸载"
        done
    done
done
finish_time=`date --date='0 days ago' "+%Y-%m-%d %H:%M:%S"`
duration=$(($(($(date +%s -d "$finish_time")-$(date +%s -d "$start_time")))))
echo "——————————————————————————————————
清理完成
共耗时 $duration 秒"
}

function check (){
if [ -f $PREFIX/bin/aapt ]; then
    if [ -n `whereis shizuku | awk '{ print $2 }'` ]; then
	echo "正在运行清理函数..." && run
    else
	echo "
	在设备中没有找到shizuku
	请前往酷安按照教程配置Termux的shizuku权限
	"
	exit 0
    fi
else
    echo "正在安装依赖 aapt 请稍候..." 
    pkg reinstall aapt -y && echo "依赖安装完成." && check
fi

}

check

```
