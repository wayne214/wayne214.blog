---
title: react-native升级0.56注意问题
date: 2018-11-13 17:40:56
tags: ['前端','ReactNative']
categories: '技术分享'
---
当前项目react-native的版本是0.53.3，因为最近在做一系列性能优化的工作，于是计划升级一下RN的版本，升级至0.56.0。
先看一下ReactNative0.56.0版本更新的内容：
https://github.com/react-native-community/react-native-releases/blob/master/CHANGELOG.md#056
步骤及遇到的问题如下：
## 1.使用https://reactnative.cn/docs/upgrading/中基于Git的自动合并更新方式：
首先全局安装react-native-git-upgrade工具模块：
```
$ npm install -g react-native-git-upgrade
```
<!-- more -->
## 2.接下来进行更新操作
```
$ react-native-git-upgrade
# 这样会直接把react native升级到最新版本

# 或者是：

$ react-native-git-upgrade X.Y.Z
# 这样把react native升级到指定的X.Y.Z版本
```
那么更新到0.56.0就是
```
$ react-naitve-git-update 0.56.0
```
## 3.等待升级结束后，需要解决冲突, 下面的这些文件的冲突都需要进行解决一下
![image.png](https://upload-images.jianshu.io/upload_images/3112038-ba13577686ed0498.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```
文件中的冲突会以分隔线隔开，并清楚的标记出处，例如下面这样：

13B07F951A680F5B00A75B9A /* Release */ = {
  isa = XCBuildConfiguration;
  buildSettings = {
    ASSETCATALOG_COMPILER_APPICON_NAME = AppIcon;
<<<<<<< ours
    CODE_SIGN_IDENTITY = "iPhone Developer";
    FRAMEWORK_SEARCH_PATHS = (
      "$(inherited)",
      "$(PROJECT_DIR)/HockeySDK.embeddedframework",
      "$(PROJECT_DIR)/HockeySDK-iOS/HockeySDK.embeddedframework",
    );
=======
    CURRENT_PROJECT_VERSION = 1;
>>>>>>> theirs
    HEADER_SEARCH_PATHS = (
      "$(inherited)",
      /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/include,
      "$(SRCROOT)/../node_modules/react-native/React/**",
      "$(SRCROOT)/../node_modules/react-native-code-push/ios/CodePush/**",
    );
上面代码中的"ours"表示你自己的代码，而"theirs"表示 React Native 的官方代码。然后你可以根据实际情况判断，选择某一方晋级，另一方出局。
```
## 注意问题
一、0.56.0以上的RN使用Babel 7
> React Native now uses Babel 7
When upgrading to 0.56, make sure to bump your babel-preset-react-native package.json dependency to 5.0.2 or newer (but still as fixed value).
React Native 现在使用 Babel 7，升级到 0.56 后，请确保将 babel-preset-react-native package.json 依赖项升级到 ^5.0.2 或更高版本。
那么就需要升级一下babel-preset-react-native版本
```
yarn add babel-preset-react-native@5.0.2 --dev
```
二、升级后遇到react native version mismatch问题
![reactnativeversionmismatch.png](https://upload-images.jianshu.io/upload_images/3112038-95dc5cf1efb878b3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

参考了https://stackoverflow.com/questions/47763824/react-native-version-mismatch中的解决方案：
1.IOS
进入项目中： 
```
cd ios && pod install
```
```
For others with the same problem on iOS with CocoaPods:

I tried all of the solutions above, without luck. I have some packages with native dependencies in my project, and some of those needed pod modules being installed. The problem was that React was specified in my Podfile, but the React pod didn't automatically get upgraded by using react-native-git-upgrade.

The fix is to upgrade all installed pods, by running cd ios && pod install.
```

2.Android
![image.png](https://upload-images.jianshu.io/upload_images/3112038-138910d1d1bb7a88.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
>第三方库使用的buildToolsVersion版本太低，不需要手动修改库内容，在android/build.gradle中添加以下内容，强制所有依赖使用相同版本。

```
subprojects {
   afterEvaluate {project ->
       if (project.hasProperty("android")) {
           android {
               compileSdkVersion 26
               buildToolsVersion '26.0.3'
           }
       }
   }
}
```
## 其他升级问题可参考这篇文章,也可在评论区与我进行沟通。
[ReactNative从0.47 升级到0.56遇到的问题](https://github.com/CodingPapi/blog/issues/1)