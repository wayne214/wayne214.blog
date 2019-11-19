---
title: React-Navigation各版本安装步骤对比
date: 2019-11-18 14:06:56
tags: "ReactNative"
---
### 1.x和2.x版本
```
yarn add react-navigation
# or with npm
# npm install --save react-navigation
```
### 3.x版本
```
yarn add react-navigation
# or with npm
# npm install react-navigation
yarn add react-native-gesture-handler react-native-reanimated
# or with npm
# npm install react-native-gesture-handler react-native-reanimated
```
1.如果你的**ReactNative版本**是**0.59**及以下，还需要手动通过link命令添加依赖
```
react-native link react-native-reanimated
react-native link react-native-gesture-handler
```
link后IOS的设置就完成了，但在Android端还需要一些配置。
对于react-native-gesture-handler这个库还需要做如下配置：
在项目根目录Android中MainActivity.java文件中，添加如下配置：
```
package com.reactnavigation.example;

import com.facebook.react.ReactActivity;
// 导入下面三个包
+ import com.facebook.react.ReactActivityDelegate;
+ import com.facebook.react.ReactRootView;
+ import com.swmansion.gesturehandler.react.RNGestureHandlerEnabledRootView;

public class MainActivity extends ReactActivity {

  @Override
  protected String getMainComponentName() {
    return "Example";
  }
// 重写createReactActivityDelegate方法
+  @Override
+  protected ReactActivityDelegate createReactActivityDelegate() {
+    return new ReactActivityDelegate(this, getMainComponentName()) {
+      @Override
+      protected ReactRootView createRootView() {
+       return new RNGestureHandlerEnabledRootView(MainActivity.this);
+      }
+    };
+  }
}
```
2.如果你项目的ReactNative版本是0.60及以上，就不用手动执行link命令添加依赖了，ios端需要执行如下命令完成link依赖, 注意需要安装Cocoapods。
```
cd ios
pod install
cd ..
```
### 4.x版本
```
yarn add react-navigation
# or with npm
# npm install react-navigation

yarn add react-native-reanimated react-native-gesture-handler react-native-screens@^1.0.0-alpha.23
```
**ReactNative 0.60**以及更高,不需要手动执行link命令，对于ios端，需要执行如下命令
```
cd ios
pod install
cd ..
```
Android端对于新添加的库：react-native-screens， 还需要如下配置，进行项目根目录android/app/build.gradle文件中,添加如下依赖：
```
implementation 'androidx.appcompat:appcompat:1.1.0-rc01'
implementation 'androidx.swiperefreshlayout:swiperefreshlayout:1.1.0-alpha02'
```
如果你的**ReactNative版本是0.59**及一下，则需要如下配置：
```
react-native link react-native-reanimated
react-native link react-native-gesture-handler
react-native link react-native-screens
```
使用androidx配置支持jetifier:
```
yarn add --dev jetifier
# or with npm
# npm install --save-dev jetifier
```
在项目的package.json文字文件添加脚本：
```
"scripts": {
  "postinstall": "jetifier -r"
}
```
之后，运行刚添加的postinstall
```
yarn postinstall
# or with npm
# npm run postinstall
```
对于Android来说， react-native-gesture-handler这个库还需要进一步配置，和3.x版本中一致，在MainActivity.java文件中导包，并重写响应方法：
```
package com.reactnavigation.example;

import com.facebook.react.ReactActivity;
+ import com.facebook.react.ReactActivityDelegate;
+ import com.facebook.react.ReactRootView;
+ import com.swmansion.gesturehandler.react.RNGestureHandlerEnabledRootView;

public class MainActivity extends ReactActivity {

  @Override
  protected String getMainComponentName() {
    return "Example";
  }

+  @Override
+  protected ReactActivityDelegate createReactActivityDelegate() {
+    return new ReactActivityDelegate(this, getMainComponentName()) {
+      @Override
+      protected ReactRootView createRootView() {
+       return new RNGestureHandlerEnabledRootView(MainActivity.this);
+      }
+    };
+  }
}
```
最后，在入口文件比如index.js 或者 App.js文件中，导入下面内容
```
import 'react-native-gesture-handler'
```
现在运行
```
react-native run-ios 或者 react-native run-android 编译并运行项目就可以了
```
最后
ReactNative开发者们，你们现在使用React-Navigation版本是多少？欢迎在文章底部留言参加讨论。


个人博客：https://blog.csdn.net/wayne214
欢迎关注个人微信公众号：君伟说。
![在这里插入图片描述](/images/个人微信公众号.jpg)