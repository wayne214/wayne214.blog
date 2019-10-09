---
layout: 开源一个图片组件
title: react-native-border-radius-image
date: 2019-10-08 15:49:52
tags: "开源组件"
categories: '开源组件'
---
大家好，十一假期biu的一下就过去了，相信大家都没有玩的尽兴，没关系，我们还有周六周日双休日(不加班)，或者再坚持三个月就到了2020年的元旦了， 是不是发现时间过得太快了。
![timg.jpeg](/images/timg.jpeg)
好了，开工了，😀。
笔者在[ReactNative之Image在Android设置圆角图片变形问题](https://www.jianshu.com/p/0079ff047bd3)这篇文章中提到了安卓端设置图片圆角的解决办法，但是还需要大家复制代码，不太友好(太麻烦，能不能npm一下，😀)。鉴于这种情况，今天做了一个开源的组件# **[react-native-border-radius-image](https://github.com/wayne214/react-native-border-radius-image)**， 大家可以试试看效果如何。欢迎大家star哦
### 使用
#### 1、安装依赖
```
yarn add react-native-border-radius-image
```
#### 2、使用
```
// 导入包
import RoundImage from 'react-native-border-radius-image'

// 页面中引用, render 方法中
// Icon 为设置的默认logo


render() {
    const imgUrl = itemData.logo_url ? {uri: itemData.logo_url} : Icon;
    return(
        <RoundImage
            source={imgUrl}
            size={40}
            style={{alignSelf: 'flex-start',
            marginTop: 2,}}
        />
    )
}
```
#### 3、有图有真相
![圆角图片.png](/images/圆角图片.png)

这样是不是更加愉快的使用了，😀。如果帮助了您，欢迎大家star。