---
title: react-native底部导航快速切换时轮播图空白
date: 2018-12-11 11:16:52
tags: 'ReactNative'
---
目前react-native平台最好用的轮播图组件：[react-native-swiper](https://github.com/leecade/react-native-swiper/)

最近在项目迭代开发测试中发现一个问题，就是在快速切换APP底部tab导航栏时，集成的轮播图组件[react-native-swiper](https://github.com/leecade/react-native-swiper/)会白屏，无法显示图片
如下图所示：
![image.png](https://upload-images.jianshu.io/upload_images/3112038-1618bbd8864b9936.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

<!-- more -->
通过查找react-native-swiper的issues发现了，也有人遇到这个问题，最终找到了解决方案：
给组件Swiper添加一个属性即可：
```
removeClippedSubviews={false}
```
添加位置如图所示：
![image.png](https://upload-images.jianshu.io/upload_images/3112038-04eddb0d1681df51.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
