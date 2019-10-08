---
title: react-native-baidu-map使用及注意问题
date: 2019-05-06 10:58:01
tags: "ReactNative"
categories: '技术分享'
---
使用组件：
# **[react-native-baidu-map](https://github.com/lovebing/react-native-baidu-map)**
## 获取百度地图API_KEY
地址：http://lbsyun.baidu.com，在控制台成功创建应用后，就可以看到应用的api key了
![image.png](https://upload-images.jianshu.io/upload_images/3112038-cce25b2b61722b9d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


## 安装
```
yarn add react-native-baidu-map
```
<!-- more -->

## 原生部分
#### Android配置
```
react-native link react-native-baidu-map
```
###### 配置AndroidManifest.xml文件
1.在<application>中加入如下代码配置开发密钥（AK）
```
<application>  
    <meta-data  
        android:name="com.baidu.lbsapi.API_KEY"  
        android:value="开发者 key" />  
</application>
```
2.在<application/>外部添加如下权限声明：
```
//获取设备网络状态，禁用后无法获取网络状态
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
//网络权限，当禁用后，无法进行检索等相关业务
<uses-permission android:name="android.permission.INTERNET" />
//读取设备硬件信息，统计数据
<uses-permission android:name="android.permission.READ_PHONE_STATE" />
//读取系统信息，包含系统版本等信息，用作统计
<uses-permission android:name="com.android.launcher.permission.READ_SETTINGS" />
//获取设备的网络状态，鉴权所需网络代理
<uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
//允许sd卡写权限，需写入地图数据，禁用后无法显示地图
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
//这个权限用于进行网络定位
<uses-permission android:name="android.permission.WRITE_SETTINGS" />
//这个权限用于访问GPS定位
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
//获取统计数据
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
//使用步行AR导航，配置Camera权限
<uses-permission android:name="android.permission.CAMERA" />
//程序在手机屏幕关闭后后台进程仍然运行
<uses-permission android:name="android.permission.WAKE_LOCK" />
```
#### IOS配置
使用pod,Podfile文件中添加
```
  pod 'React', :path => '../node_modules/react-native', :subspecs => [
    'Core',
    'CxxBridge',
    'DevSupport', 
    'RCTText',
    'RCTNetwork',
    'RCTWebSocket', 
    'RCTAnimation'
  ]
  pod 'yoga', :path => '../node_modules/react-native/ReactCommon/yoga'
  pod 'DoubleConversion', :podspec => '../node_modules/react-native/third-party-podspecs/DoubleConversion.podspec'
  pod 'glog', :podspec => '../node_modules/react-native/third-party-podspecs/glog.podspec'
  pod 'Folly', :podspec => '../node_modules/react-native/third-party-podspecs/Folly.podspec'

  pod 'react-native-baidu-map', :podspec => '../node_modules/react-native-baidu-map/ios/react-native-baidu-map.podspec'
```
#### 注意！！！：AppDelegate.m init 初始化，使用如下代码，可以解决RCTBaiduMapViewManager.h文件找不到的问题
```
#import <react-native-baidu-map/BaiduMapViewManager.h>
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
{
    ...
    // 地图 ak 注册
    [BaiduMapViewManager initSDK:@""];
    ...
}
```
## 使用
```
导入
import { MapView, MapTypes, Geolocation, Overlay } from 'react-native-baidu-map'
const { Marker } = Overlay;

<MapView
         width={deviceWidth}
         height={200}
         zoom={18}
         trafficEnabled={true}
         zoomControlsVisible={true}
         mapType={MapTypes.SATELLITE}
         center={{ longitude: 116.465175, latitude: 39.938522 }}
>
        <Marker
            title='中心'
            location={{longitude: 116.465175, latitude: 39.938522}}
          />
</MapView>
```
## 效果，上图
![image.png](https://upload-images.jianshu.io/upload_images/3112038-3e4b2899b97411ca.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
