---
title: ReactNative之Image在Android设置圆角图片变形问题
date: 2018-11-02 17:44:59
tags: 'ReactNative'
categories: '技术分享'
comments: true
---
ReactNative中的Image使用时比较简单的，比如下面这样：
```
<Image
            resizeMode='contain'
            defaultSource={require('images/avatar_placeholder.png')}
            source={{ uri: 
                    'http://img1.qimingpian.com/product/raw/2b7285ee83af426c321002e27247377a.jpg' }}
            style={{width: 50, height: 50}}
        />
```
效果就是这样了
![image.png](https://upload-images.jianshu.io/upload_images/3112038-504d6671b0382abd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

问题来了，如果给Image设置了圆角了话，Android上就显示有问题了，
<!-- more -->

```
<Image
            resizeMode='contain'
            defaultSource={require('images/avatar_placeholder.png')}
            source={{ uri:
                    'http://img1.qimingpian.com/product/raw/2b7285ee83af426c321002e27247377a.jpg' }}
            style={{
              width: 50, height: 50,
              borderWidth: StyleSheet.hairlineWidth,
              borderRadius: 3,
              borderColor: color.STARUP.LINE_BACKGROUND}}
        />
```
就会出现下面的图片变形问题，图片在安卓手机上会出现多余的颜色
![image.png](https://upload-images.jianshu.io/upload_images/3112038-69c1c872fe63e1ab.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

怎么解决他呢？
如果需要给图片加圆角，解决方案如下：
## 1.Image不设置圆角，外面用View包裹一下，设置View的圆角
```
<View style={{
            width: 50, height: 50,
            borderWidth: StyleSheet.hairlineWidth,
            borderRadius: 3,
            borderColor: color.STARUP.LINE_BACKGROUND}}>
          <Image
              resizeMode='contain'
              defaultSource={require('images/avatar_placeholder.png')}
              source={{ uri:
                      'http://img1.qimingpian.com/product/raw/2b7285ee83af426c321002e27247377a.jpg' }}
              style={{width: 50, height: 50,}}
          />
        </View>
```
##2.设置overlayColor的颜色
```
<Image
            resizeMode='contain'
            defaultSource={require('images/avatar_placeholder.png')}
            source={{ uri:
                    'http://img1.qimingpian.com/product/raw/2b7285ee83af426c321002e27247377a.jpg' }}
            style={{
              width: 50, height: 50,
              borderWidth: StyleSheet.hairlineWidth,
              borderRadius: 3,
              borderColor: color.STARUP.LINE_BACKGROUND,
              overlayColor: '#ffffff'
            }}
        />
```
ok,就酱自了。

都怪自己读书少，没好好看文档：
![image.png](https://upload-images.jianshu.io/upload_images/3112038-06d517f19f9c6248.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

