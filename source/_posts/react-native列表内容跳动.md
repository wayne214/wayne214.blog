---
title: 关于ReactNative0.56版本Flatlist列表内容跳动的问题
date: 2019-01-04 12:22:07
tags: 'ReactNative'
---
Reactnative的版本升级一直是一个工作量比较的大的事情，每次升级都可能伴随着很多的坑。
前段时间在升级到0.56版本的时候发现一个问题，在flatlist使用中，加载多页后，列表项内容开始进行上下抖动的乱跳，疯了一样。
于是开始上react-native的issues上寻找答案，有通过查看官方的版本升级日志找到了答案：[react-native升级日志0.57](https://github.com/react-native-community/react-native-releases/blob/master/CHANGELOG.md#057)
在其中看到如下bugFix描述：
<!-- more -->
![image.png](https://upload-images.jianshu.io/upload_images/3112038-b5fb47511dc3c4c2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

因为Flatlist继承自VirtualizedList,所以就豁朗开朗了。
解决方案：将ReactNative Version升级到0.57以上版本就好了
[官方升级方案](https://reactnative.cn/docs/upgrading/),推荐第一种“基于 Git 的自动合并更新”的升级方案。
