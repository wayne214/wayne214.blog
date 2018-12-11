---
title: ReactNative集成百度语音合成
date: 2018-10-31 17:11:32
tags: 'ReactNative'
categories: '技术分享'
---
语音交互是现今应用最多的智能交互方式，在人工智能越来越火的当下应用十分广泛，所以特别针对车内环境，在驾驶员安心驾驶的时候，用语音可以安全的进行操控。恰好新版项目中要加入语音播报功能，因为我们的应用和司机有关，于是在网上搜索一些解决方案，目前有阿里云，百度云以及科大讯飞还有一些其他公司提供的相关解决方案。
不同方案之间的对比，可以参考下面的文章：
[智能语音方案比对介绍](http://blog.csdn.net/rui1605/article/details/74391341)
http://www.jianshu.com/p/95d95f8189aa

目前计划采用的是百度云提供的语音合成技术：
有如下几个优势：
<!-- more -->

1.支持多种语言多种音色
支持中文、英文混读，男声、女声、童声、情感男声可供选择，更支持语速、音调、音量、音频码率设置，让应用拥有最甜美和最磁性的声音
2.支持离线在线融合模式
SDK可以根据当前网络状况及指令的类型，自动判断使用本地引擎还是云端引擎进行语音合成
3.合成效果流畅自然
语音合成技术业界领先，合成效果接近
真人发声，流畅自然，且极具表现力，
给你最舒适的听觉体验
4.免费额度高

![image.png](http://upload-images.jianshu.io/upload_images/3112038-b4a33d488eea8fd8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

好了，不给它打广告了，开始整干货，下面是集成步骤。
先贴上百度云官网文档地址：http://ai.baidu.com/docs#/TTS-Android-SDK/top
1.创建一个ReactNative工程，不会的自行百度吧；或者在已有项目中，总之你得有个RN项目
2.登录网址百度语音开发者平台注册账号并创建应用：
进入控制台-->选择产品服务-->选择人工智能-->创建应用-->填写有关应用信息
![image.png](http://upload-images.jianshu.io/upload_images/3112038-805498c45751537a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![image.png](http://upload-images.jianshu.io/upload_images/3112038-316e4c8e2b820498.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![image.png](http://upload-images.jianshu.io/upload_images/3112038-5f61cc4542f0b2bd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![image.png](http://upload-images.jianshu.io/upload_images/3112038-606981925949b433.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![image.png](http://upload-images.jianshu.io/upload_images/3112038-bfbd47fbe363c5eb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
同理点击查看Key，查看当前应用的所需的主要三个参数 AppId APIKey SecretKey，后面会用得到.

3.下载相关平台的SDK

![image.png](http://upload-images.jianshu.io/upload_images/3112038-cb5ece9ecc86b6dc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

4.解压后

![image.png](http://upload-images.jianshu.io/upload_images/3112038-c5a97e9c267cd32a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
BaiduTtsSample:为一个模板代码，eclipse版本的，我就是借鉴里面稍微修改了一下。 
 data:为百度语音资源，声音文件，它为一个必须文件，中英文资源。最后使用是放在手机物理存储下的。 
 doc:为一个pdf的简介使用方法以及网络的使用Api文档说明。我们用不到，可以下去读一读的。 
 libs:为资源jar包和语音引擎文件.so库。也是我们集成必须使用到的。
5.接下来的步骤是，我们将语音资源和libs下的资源方法android studio我们的项目里面。将data里面的文件全部复制到Asserts文件夹下。将libs下的两个jar文件复制到项目的libs中,并添加Add As library关联。在项目中的main路径下新建一个jnilibs文件夹，用于放置剩余的libs下的文件。现在的工程目录是(Android)

![image.png](http://upload-images.jianshu.io/upload_images/3112038-6ad66a964a818c0b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

6.添加权限
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.WRITE_SETTINGS" />
<uses-permission android:name="android.permission.READ_PHONE_STATE" />
<uses-permission android:name="android.permission.MODIFY_AUDIO_SETTINGS" />
<uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
<uses-permission android:name="android.permission.CHANGE_WIFI_STATE" />

7.到此，集成就结束了，接下来就是如何使用。当然可以参照BaiduTtsSample文件夹下的src里面的一个MainActvity的写法。也可以按照下面的总结的工具类来直接使用，方便快捷省事。
参考文章：http://blog.csdn.net/bk120/article/details/54020505
--------------------------华丽的分割线----------------------------
因为咱们的的项目是ReactNative，所以要进行原生和js的交互
1.创建一个原生模块是一个继承了ReactContextBaseJavaModule的Java类，它可以实现一些JavaScript所需的功能。

![image.png](http://upload-images.jianshu.io/upload_images/3112038-f4fe4dab3372e1e9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

2.注册模块

![image.png](http://upload-images.jianshu.io/upload_images/3112038-bafb78d1f831a774.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

3.在这个package需要在MainApplication.java文件的getPackages方法中提供。这个文件位于你的react-native应用文件夹的android目录中。

![image.png](http://upload-images.jianshu.io/upload_images/3112038-3cca14ada84e5d57.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

4.为了让你的功能从JavaScript端访问起来更为方便，通常我们都会把原生模块封装成一个JavaScript模块。这不是必须的，但省下了每次都从NativeModules中获取对应模块的步骤。这个JS文件也可以用于添加一些其他JavaScript端实现的功能。


![image.png](http://upload-images.jianshu.io/upload_images/3112038-ad3119354d17757a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

5.使用

![image.png](http://upload-images.jianshu.io/upload_images/3112038-5141dc64b2ab574c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![image.png](http://upload-images.jianshu.io/upload_images/3112038-977360e0c2f0618d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

6.集成常见问题：
百度语音文档中心：http://yuyin.baidu.com/docs/tts/84

Oc 百度语音的ios集成:http://blog.csdn.net/qq_40691827/article/details/78333380
iOS 一行代码简单调用百度语音合成:http://www.jianshu.com/p/1c4a3f248098

ps:使用Android原生自带的语音合成：
https://github.com/SolveBugs/Utils/blob/master/SpeechUtils.java
https://blog.csdn.net/csdn_blog_lcl/article/details/52504323
