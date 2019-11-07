---
layout: flutter
title: Flutter build failed android
date: 2019-10-25 17:48:58
tags: 'Flutter'
categories: '技术分享'
---
笔者作为小白入坑Flutter,在Flutter写了一个入门demo,在进行Android编译的时候报错code如下：
```
Daemon:  AAPT2 aapt2-3.2.1-4818971-osx Daemon #0
```
具体报错截图如下：
![Flutter编译报错.png](/images/Flutter编译报错.png)
### 解决方案：
进入项目工程的android/app/build.gradle文件，修改编译的sdk版本为28,重新编译即可。
![build修改内容.png](/images/build修改内容.png)
代码如下：
```
compileSdkVersion 28
```
欢迎关注个人公众号：君伟说。
个人网站：https://wayne214.github.io
一个有温度的公众号，扫码关注哦。
![个人微信公众号.jpg](/images/个人微信公众号.jpg)
