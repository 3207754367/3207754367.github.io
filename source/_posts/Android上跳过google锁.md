---
title: Android上跳过google锁
categories: Android教程
abbrlink: 48f1e393
date: 2020-05-17 09:18:22
comments:
description: 跳过这该死的frp帐号锁
---
<!--more-->
安装过Gapps套件的小伙伴应该都知道Google开机引导锁,  这其实是因为android的frp机制(factory reset protection)导致的,  android手机恢复出厂设置的操作分为信任和不信任操作: 在解锁装态通过设置里的恢复出厂设置选项进行重置的操作,  属于信任操作.  所以, 在关机状态下通过Fastboot或recovery中进行的重置操作, 就属于不信任操作.  而使用不信任操作进行恢复出厂设置, 开机就会触发frp机制导致设备被锁. 现在让我们来跳过这烦人的验证吧！

禁用出厂重置保护:

## 开机状态下退出Google账户

**避免进行不信任操作, 在刷机前就将Google帐号从系统中移除.**

使`出厂重置保护(Factory Reset Protection)`变成禁用状态.  如果要禁用出厂重置保护, DPC必须设置`disableFactoryResetProtectionAdmin`键值为`true`

```bash
`DevicePolicyManager.setApplicationRestrictions()` 中的
`disableFactoryResetProtectionAdmin`默认值为`false`
 禁用FRP只需要将值改为`true`就行了. 
DPC根据它的值来发送广播通知Google Play服务
`com.google.android.gms.auth.FRP_CONFIG_CHANGED` 作出相应改变
```
相关文档: [google开发者文档](https://developer.android.google.cn/work/dpc/security?hl=fi#disable_factory_reset_protection)



## 顺时针点屏幕四角法

这是Gapps套件专属过引导方法，连点屏幕四角就可跳过此引导（**顺时针**方向, 从左上角开始.  Google官方像素系的 Google Service Framework 没有此功能

## 更改USER_SETUP_COMPLETE和DEVICE_PROVISIONED

GoogleSetupWizard完成引导后会把这俩的值由0改为1, 所以我们先它一步在它前面把这俩值改成1就好了

- 打开命令行窗口输入以下命令

```bash
adb shell settings put secure user_setup_complete 1
adb shell settings put global device_provisioned 1
adb shell reboot
```

或者也可以直接删除`system/priv-app/SetupWizard`, 如果系统原来自带的Provision没有被Google家的SetupWizard覆盖, SetupWizard被删除后Provision会把这两个值直接设为1.

```bash
Provision：

// Add a persistent setting to allow other apps to know the device has been provisioned.
Settings.Global.putInt(getContentResolver(), Settings.Global.DEVICE_PROVISIONED, 1);
Settings.Secure.putInt(getContentResolver(), Settings.Secure.USER_SETUP_COMPLETE, 1);
```



# 清空 config 或 frp 分区

谷歌锁的状态是存放在这个分区上的，那我们只要把这个分区清空就好了。

由于此分区不存在文件系统，清空的方法是往这个分区里填满0即可。

```bash
dd if=/dev/zero of=/dev/block/bootdevice/by-name/config
```
或者: 

```bash
dd if=/dev/zero of=/dev/block/bootdevice/by-name/frp
```

还可以通过`fastboot erase frp` 或 `fastboot erase config` 命令来清除谷歌锁



