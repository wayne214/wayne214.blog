---
layout: react
title: React Native 0.58 正式版发布
date: 2019-01-25 15:45:02
tags: "ReactNative"
categories: '科技资讯'
---
原文地址：https://github.com/react-native-community/react-native-releases/blob/master/CHANGELOG.md#0580
本文由简书作者[凌宇之蓝](https://www.jianshu.com/u/8ba7c349861d)翻译,因本人水平有限，难免翻译有误，还望各位见谅。
##[0.58.0]
欢迎阅读2019年1月发布的React Native。此版本有许多重大变化，我们特别提请您注意：
- 核心组件的流程类型的现代化和加强
- 中断对ScrollView，CameraRollView和SwipeableRow的更改，使其在某些方法中不再绑定到组件实例
- 支持WebKit中的相互TLS
- 从/ assets之外的目录提供的资产
- 针对意外行为的大量崩溃修复和解决方案

感谢那些对我们的发布候选人提供反馈的人。如果您有兴趣帮助评估我们的下一个版本，请在此处查看我们的跟踪问题。
<!-- more -->
##新增
- 添加对publicPath的支持以启用来自不同位置的静态资产（0b31496 by @gdborton）
####Android
- 现在可以使用Android系统属性设置Bundler服务器主机，以便在多个应用程序或应用程序安装中更轻松地进行调试adb shell setprop metro.host（@stepanhruda的e02a154）
- Native Modules现在可以使用额外的属性（userInfo）附加WritableMap arg来拒绝承诺。请参阅Promise.java中定义的接口以获取可用的方法。这可以在JavaScript中以Error.userInfo形式访问。这是为了匹配iOS现有的Error.userInfo行为。有关示例，请参阅PR。（@Salakar＃20940）
- Native Modules现在将nativeStackAndroid属性暴露给使用Exception / Throwable拒绝的promise  - 使Javascript内的本机错误堆栈可用：Error.nativeStackAndroid。这是为了匹配iOS现有的Error.nativeStackIOS支持。有关示例，请参阅PR。（@Salakar＃20940）
####IOS
- 将moduleForName：lazilyLoadIfNecessary添加到RCTBridge.h以按名称查找模块并强制加载它们，以及对@dhahidehpour，@ fkgozali和@mmmulani进行的LazyLoading的各种改进
- 当使用WebKit = {true}进行相互TLS身份验证时，将WebView的功能添加到setClientAuthenticationCredential（8911353 by @mjhu）
##Changed
- 核心组件的Flow类型的主要改进
- 许多公共组件都转换为ES6类
- Flow依赖现在为v0.86.0
- metro依赖现在是v0.49.1
- jest依赖现在是v24.0.0-alpha.6
- fbjs-scripts依赖现在是v1.0.0（＃21880）
- folly的依赖现在是v2018.10.22.00
- React sync for revisions
- 热重新加载时清理的错误消息
- 允许CxxModules实现需要两次回调的函数
###突破性变化
- 转换为ES6类的组件的公共方法不再绑定到其组件实例。对于ScrollView，受影响的方法是setNativeProps，getScrollResponder，getScrollableNode，getInnerViewNode，scrollTo，scrollToEnd，scrollWithoutAnimationTo和flashScrollIndicators。对于CameraRollView，受影响的方法是：rendererChanged。对于SwipeableRow，受影响的方法是：close。因此，通过引用将这些方法作为回调传递给函数已不再安全。组件实例的自动绑定方法是createReactClass的一种行为，我们决定在切换到ES6类时不保留这种行为。
####Android
- 优化PlatformConstants.ServerHost，PlatformConstants.isTesting和PlatformConstants.androidID以获得性能
####IOS
- 禁止关于本地模块缺少导出的黄色框

##移除
- 移除 UIManager.measureViewsInRect()
##修复bug
- 修复Yoga JNI绑定中潜在的UI线程停顿方案
- 修复因桥接cxx模块注册表周围的竞争条件而发生崩溃的问题
- 修复视图和文本的displayName;显示特定名称而不是通用“组件”
- 修复react-native init --help，使其不返回undefined
- 修复react-native --sourceExts
- 修复当可见道具未定义或为空时意外显示模态
- 修复VirtualizedList分页期间的崩溃
- 修复使用远程调试和Delta捆绑包删除模块可能导致堆栈跟踪不正确的情况
####Android具体修复bug:
- 删除根节点时修复崩溃
- 修复各种ReactInstanceManager死锁和竞争条件
- 解除ReactModalHostView和DialogManager时修复IllegalArgumentException
- 使用Android Gradle Plugin 3.2修复不正确的合并资产路径
- 在onoutout回调时修复HTTP连接
- 当远程服务器启动关闭时，修复websocket正确关闭
- 修复Android 16设备的兼容性问题
- 修复了在加载源时不遵守Image.resizeMode的问题，从而导致意外填充
- 修复Android 28的倒置ScrollView，使动量处于正确的方向
####IOS具体修复bug:
- 修复内联视图内容未被重新传输的情况
- 修复使用前置摄像头时ImagePickerIOS图像不一致的问题
- 修复竞争条件并在关闭iOS 11及更早版本的JSC时崩溃
- 修复NetInfo的_firstTimeReachability中的崩溃
- 修复内联视图可见的情况，即使它应该被截断
- 使用与内容偏移相关的ScrollView修复崩溃