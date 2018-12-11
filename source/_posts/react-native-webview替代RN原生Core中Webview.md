---
title: react-native-webview替代RN原生Core中Webview
date: 2018-12-04 15:30:15
tags: 'ReactNative'
---
> **Warning** Please use the [react-native-community/react-native-webview](https://github.com/react-native-community/react-native-webview) fork of this component instead. To reduce the surface area of React Native, `<WebView/>` is going to be removed from the React Native core. For more information, please read [The Slimmening proposal](https://github.com/react-native-community/discussions-and-proposals/issues/6).

一句话就是：使用 react-native-community/react-native-webview 替代。<WebView/> 要被从react-native core中删除了。

现在[react-native-webview](https://github.com/react-native-community/react-native-webview)现在支持Android平台的onShouldStartLoadWithRequest方法了


<!-- more -->

##使用方式：
```
$ yarn add react-native-webview
$ react-native link react-native-webview
```
##使用案例：
```
import React, { Component } from "react";
import { StyleSheet, Text, View } from "react-native";
import { WebView } from "react-native-webview";

// ...
class MyWebComponent extends Component {
  render() {
    return (
      <WebView
        source={{ uri: "https://infinite.red/react-native" }}
        style={{ marginTop: 20 }}
        onLoadProgress={e => console.log(e.nativeEvent.progress)}
      />
    );
  }
}
```
如果出现如下报错：
>Invariant Violation: requireNativeComponent: "RNCWKWebView" was not found in the UIManager

解决方案：
###1.将RNCWebView.xcodeproj添加到项目的Libraries中， 
用Xcode打开项目，在librarie右键点击add Files to 项目中
![image.png](https://upload-images.jianshu.io/upload_images/3112038-188402bc275c6971.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
###2.在Build Phases的Link Binary With Libraries中添加libRNCWebView.a文件
![image.png](https://upload-images.jianshu.io/upload_images/3112038-552ba1284076d247.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)