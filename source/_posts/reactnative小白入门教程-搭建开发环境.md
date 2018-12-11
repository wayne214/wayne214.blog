---
title: reactnative入门教程-搭建开发环境
date: 2018-11-08 11:17:45
tags: 'ReactNative'
categories: '技术分享'
---
俗话说“工欲善其事，必先利其器”，这对咱们程序猿来说更是如此，强大的开发利器，不仅有助于我们提高开发效率，更能够让我们的开发过程变的舒心。这里我墙裂推荐大家配置一个Mac Book Pro,这简直是程序开发者的神奇。免费打个广告吧：
https://item.jd.com/7842699.html, 现在适逢双11，更有优惠活动。
正式进入主题。
## 配置开发环境
这里直接推荐ReactNative中文网的配置说明：
https://reactnative.cn/docs/getting-started.html

## 开发IDE
响应平台开发软件： Xcode，Android Studio
Git命令行优化：[item命令控制面板配色](https://www.jianshu.com/p/b8b02a5d3212)
这里推荐大家使用[WebStorm](https://www.jetbrains.com/webstorm/) 进行ReactNative的开发，非常强大的IDE，智能提醒，语法高亮等。
使用Windows系统的小伙伴，配置开发环境可参考这篇文章：
[React Native开发一 webstorm搭建React Native开发环境](https://blog.csdn.net/qiyei2009/article/details/78820207)
## 注意事项
！！！注意！！！：init 命令默认会创建最新的版本，而目前最新的 0.45 及以上版本需要下载 boost 等几个第三方库编译。这些库在国内即便翻墙也很难下载成功，导致很多人无法运行iOS项目！！！中文网在论坛中提供了这些库的国内下载链接。如果你嫌麻烦，又没有对新版本的需求，那么可以暂时创建0.44.3的版本。
```
react-native init youprogectname
```
开在如下表中选择相应的版本号，进行创建项目，
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181108103114696.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dheW5lMjE0,size_16,color_FFFFFF,t_70)
比如
```
react-native init reactnativeDemo --version 0.44.3
```
>提示：你可以使用--version参数（注意是两个杠）创建指定版本的项目。例如react-native init MyApp --version 0.44.3。注意版本号必须精确到两个小数点。

如果在配置环境的过程中，遇到什么问题，可以在博客下方评论区给我留言，我会第一时间进行回复。