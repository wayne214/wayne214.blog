---
layout: intellij
title: IntelliJ IDEA创建第一个Spring boot项目
date: 2019-01-21 09:57:28
tags: "Java Web"
---
首先下载maven：http://maven.apache.org/download.cgi
![image.png](https://upload-images.jianshu.io/upload_images/3112038-4555d277efa67639.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

开发工具：[IntelliJ IDEA](https://www.jetbrains.com/idea/?fromMenu)
JDK:  [Java JDK1.8](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)


<!-- more -->
## 1.为了第一个项目初始化速度加快，我们先来配置maven:
添加配置：
![image.png](https://upload-images.jianshu.io/upload_images/3112038-9b492a448ebd8236.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
选择Build,Execution,Deployment下， Bulid Tools下的Maven,在勾选右边红框中的Override,
![image.png](https://upload-images.jianshu.io/upload_images/3112038-5448396ea87e2d17.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
选择你下载后的文件夹中的settings.xml
![image.png](https://upload-images.jianshu.io/upload_images/3112038-b983c329d7dded95.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
## 2.使用IntelliJ IDEA创建springboot项目
1.创建新项目
![image.png](https://upload-images.jianshu.io/upload_images/3112038-1cadbe84ee621282.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
2.选择spring，选择jdk1.8
![image.png](https://upload-images.jianshu.io/upload_images/3112038-698bc1294c7ae9a9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
3.填写group ，选择packaging--- War, 选择Next
![](https://upload-images.jianshu.io/upload_images/3112038-4a7fd4d0e71799f8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
4.选择Web, 点击Next,下一步点击finish就好了。
![image.png](https://upload-images.jianshu.io/upload_images/3112038-b826e74d6dea554f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
5.等着项目初始化完成就可以了。
6.在项目的applicaiton右键，选择Run "DemoApplication"
![image.png](https://upload-images.jianshu.io/upload_images/3112038-4425bdd732f54e79.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

运行成功的截图：
![image.png](https://upload-images.jianshu.io/upload_images/3112038-32428c7ecd16d073.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我的网站：https://wayne214.github.io