---
title: Taro小程序采坑记
date: 2018-12-24 17:46:16
tags: '小程序'
---
[Taro](https://taro.aotu.io/)，京东凹凸实验室出品的适配多端的一个框架，
**Taro** 是一套遵循 [React](https://reactjs.org/) 语法规范的 **多端开发** 解决方案。现如今市面上端的形态多种多样，Web、React-Native、微信小程序等各种端大行其道，当业务要求同时在不同的端都要求有所表现的时候，针对不同的端去编写多套代码的成本显然非常高，这时候只编写一套代码就能够适配到多端的能力就显得极为需要。

使用 **Taro**，我们可以只书写一套代码，再通过 **Taro** 的编译工具，将源代码分别编译出可以在不同端（微信小程序、H5、RN 等）运行的代码。

>But 理想很丰满，现实很骨感
最近在尝试采用其编写小程序代码，发现采坑的地方不少
<!-- more -->
##事件处理bind函数，不能传值了？
>Taro 目前暂时不支持通过匿名函数传值，也不支持多层 lambda 嵌套。当你有传参需求时，请全部使用 bind 来处理。
更新了@tarojs/cli为最新版后，发现bind的方法不能传值了
```
<Button onClick={this.goto.bind(this,'111')}>跳转详情页</Button>
```
![image.png](https://upload-images.jianshu.io/upload_images/3112038-4bb083d1562e2210.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
打印出来的是这个鬼：
![image.png](https://upload-images.jianshu.io/upload_images/3112038-46f62a35644b79ce.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
根本不是传递的字符串
根据issues中提供的方式：
>cli 和项目依赖都要升级到 1.2.1
使用命令行更新cli及项目依赖后能够正常传值了
```
taro update project
```