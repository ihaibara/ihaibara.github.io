---
layout: post
title: 检测Windows进程是否是提权运行状态
categories: Windows
description: 本文描述了如何使用任务管理器查看某个正在运行的进程是否是处于提权运行状态
keywords: Windows
---

*本文描述了如何使用任务管理器查看某个正在运行的进程是否是处于提权运行状态*

# 检测Windows进程是否是提权运行状态

> 本文主要引用了[How to check if a process is running as administrator (elevated) in Windows](http://winaero.com/blog/how-to-check-if-a-process-is-running-as-administrator-elevated-in-windows/)

## 背景

自从Windows Vista引入了用户帐户控制以来，有必要偶尔作为管理员运行一些程序进行一些功能。 如果UAC设置设置为Windows中的最高级别，那么当您以管理员身份打开应用程序时，您将获得UAC提示。 但是，当UAC设置处于较低级别时，签名的Windows EXE将以静默方式提升自身的运行权限（elevated）。 此外，还有一些作为管理员运行的计划任务，*您甚至可以创建自己的快捷方式来在没有UAC提示的情况下提升运行*。 在本文中，我们将看到如何确定进程是否以管理员身份运行。

## Windows 10/Windows 8.1/Windows 8

- 打开任务管理器，切换到详细信息tab页面，一般情况下如图所示

![win10-taskmgr-normal](http://otsjxkilz.bkt.clouddn.com/markdown-img-paste-2017072815565359.png)

- 在任意列上右击，在弹出的”选择列”中下拉选择“特权”列

![win10-taskmgr-select](http://otsjxkilz.bkt.clouddn.com/markdown-img-paste-2017072816025301.png)

- 然后就可以看到目前处于提权状态的进程了，比如我们的360

![win10-task-mgr-elevated](http://otsjxkilz.bkt.clouddn.com/markdown-img-paste-20170728160431928.png)

## Windows 7 or Windows Vista

在Win 7中，上面提到的选择列中，并没有“特权”列，这个时候我们选择“UAC虚拟化”

![win7-uac-vitual](http://otsjxkilz.bkt.clouddn.com/markdown-img-paste-2017072816081552.png)

我没有在Win 7上测试过，不过在Win10 上，多数的结果和特权的显示是一致的

![win10-win7-uav-virtual](http://otsjxkilz.bkt.clouddn.com/markdown-img-paste-20170728160939904.png)
