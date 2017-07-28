---
layout: post
title: 在不弹出UAC提示的情况下以管理员权限运行程序
categories: Windows
description: 本文描述了如何使用计划任务在不弹出UAC提示的情况下以管理员权限运行程序
keywords: Windows
---

*本文描述了如何使用计划任务在不弹出UAC提示的情况下以管理员权限运行程序*

# 在不弹出UAC提示的情况下以管理员权限运行程序

## 背景

用户帐户控制（UAC）是与Microsoft Windows Vista和Windows Server 2008操作系统一起引入的技术亦即安全基础设施，Windows 7，Windows Server 2008 R2，Windows 8，Windows Server 2012和Windows 10等均继承了这项技术。它旨在通过将应用程序软件限制为标准用户权限，直到管理员授权增加或升高来提高Microsoft Windows的安全性。 以这种方式，只有用户信任的应用程序才会拥有管理权限，恶意软件应该不会损害操作系统。

**UAC的确有效地保护了用户的操作系统，作为一个正常运行的应用程序，在任何情况下都不应该尝试绕过系统的UAC保护**

*在某种特殊情况下我们可以通过创建、运行任务计划来以不弹出UAC提示的情况下提权运行程序*

## 创建计划任务

- 我们需要进入到*计划任务*管理的界面，比如我们从控制面板中进入

![enter-schedule-from-control-panel](http://otsjxkilz.bkt.clouddn.com/markdown-img-paste-20170728164655271.png)

> 可以看到，计划任务旁边是有小盾牌图标的，也就是说，你在更改计算任务时候是需要拥有管理员权限的，不过这个是所谓的*Windows设置*，默认UAC设置下不会有UAC权限提示
> ![](http://otsjxkilz.bkt.clouddn.com/markdown-img-paste-20170728164914536.png)

- 在*任务计划程序*中，选择左侧的*任务计划程序库*，可以看到许多任务计划，比如我们的大LM，360。选择右边的*创建任务*可以开始创建一个计划任务

![schedule-task-window](http://otsjxkilz.bkt.clouddn.com/markdown-img-paste-20170728165052274.png)

- 在*创建任务*界面中的*常规*选项卡中，我们需要设置任务的名称，比如NotePad，然后在下面勾选*使用最高权限运行*

![Create-task-general-tab](http://otsjxkilz.bkt.clouddn.com/markdown-img-paste-20170728165654229.png)

- 在*创建任务*界面中的*触发器*选项卡中，我们可以为任务添加一个触发的条件，***如果设置了登录时选项，那么每次用户登录的时候，就会触发我们设定的任务计划了***，不过为了测试方面，我们就不添加任何触发器了

![Create-task-trigger-tab](http://otsjxkilz.bkt.clouddn.com/markdown-img-paste-20170728170002978.png)

- 在*创建任务*界面中的*操作*选项卡中，在左下角我们可以新建一个任务的动作，比如我这里设置了打开notepad++

![Create-task-operation-tab](http://otsjxkilz.bkt.clouddn.com/markdown-img-paste-20170728170314302.png)

- 其他的设置目前和本文的目的无关，读者可以自行查看、设置。

- 确定创建任务之后，我们就可以在任务列表中看到我们创建的任务计划了

![create-task-done](http://otsjxkilz.bkt.clouddn.com/markdown-img-paste-20170728170638408.png)

## 运行计划任务

- 在桌面或者文件夹空白处右击，新建快捷方式

![new-shortcut](http://otsjxkilz.bkt.clouddn.com/markdown-img-paste-20170728170928706.png)

- 在*对象的位置*中输入以下字符串，其中NotePad需要替换成刚刚的计划任务名字

> C:\Windows\System32\schtasks.exe /run /tn NotePad

- 点击*下一步*，给快捷方式起个名字，确定就好了，比如下面这个样子

![new-shortcut-name](http://otsjxkilz.bkt.clouddn.com/markdown-img-paste-20170728171328360.png)

- 然后启动这个快捷方式，程序会在一个黑框~~schtasks.exe：怪我咯~~闪过之后以权限提升方式运行

![exe-run-elevated](http://otsjxkilz.bkt.clouddn.com/markdown-img-paste-20170728171643862.png)

***本文完***
