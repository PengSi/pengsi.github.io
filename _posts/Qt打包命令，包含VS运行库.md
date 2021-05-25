---
layout: post
title: "Qt打包命令，包含VS运行库"
date: 2021-03-09
description: ""
categories: Qt
tags: [Qt,VS]
---


```shell
windeployqt --qmldir D:\Qt\Qt5.15.2\5.15.2\msvc2019_64\qml TemplateProject.exe
```
用VS的命令行执行在对应的exe目录执行windeployqt ，如果执行成功，会将vcredist_x86.exe拷贝到当前目录下，对于用到的其他第三方库，需要手动将对应的dll拷贝到可执行程序目录，否则程序运行时会出现缺少相关dll的错误
