---
title: ReactNative入门教程-项目结构解析及HelloWorld
date: 2018-11-22 11:46:41
tags: 'ReactNative'
---
今天我们从历史传统“Hello World”开始。

## 首先创建一个项目, 指定创建0.55.4的版本

```
react-native init rndemo --version 0.55.4 
```

## 进入项目中，使用命令yarn install 安装依赖
![](https://oscimg.oschina.net/oscnet/33ff8f674d7358e667e3da92d8ec31a49df.jpg)

等待安装完成之后，进入项目根目录，使用如下命令运行到iOS或Android模拟器上，即可看到下面的画面，就表示运行项目成功了
```
react-native run-ios or run-android
```
![](https://oscimg.oschina.net/oscnet/7bcc6146a133442c64b3be04e8118360e7c.jpg)

## 项目的目录结构如下：
![](https://oscimg.oschina.net/oscnet/5242b45502e6def729fc51f85e3fa6dd2e3.jpg)

咱们接下来逐一解释一下目录结构的组成部分：

## 项目结构解析

 - android: Android的原生开发文件目录，用Android Studio打开项目就是打开这个文件进行原生开发
 - ios: IOS原生开发目录，使用Xcode打开进行原生开发
 - node_modules: 使用“npm install”或者“yarn install”，根据项目中package.json配置自动生成的项目依赖库
- .babelrc: Babel的配置文件,Babel是一个广泛使用的转码器，主要用来将ES6代码转为ES5代码，从而在现有环境执行。babelrc用来设置转码规则和插件。

- .buckconfig: buck的配置文件，buck是Facebook推出的一款高效率的App项目构建工具。

- .flowconfig: Flow 是 Facebook 旗下一个为 JavaScript 进行静态类型检测的检测工具。它可以在 JavaScript 的项目中用来捕获常见的 bugs，比如隐式类型转换，空引用等等。

- .gitattributes: git属性文件设定一些项目特殊的属性。比如要比较word文档的不同；对strings程序进行注册；合并冲突的时候不想合并某些文件等等。

- .gitignore: 用来配置git提交需要忽略的文件。

- .watchmanconfig: 用于监控bug文件和文件变化，并且可以出发指定的操作。

- App.js：就是编写代码的地方，稍后“hello world”就是在这里进行修改。

- app.json: 配置了name和displayName，不过没发现在哪里使用到了。（待研究。。）
- index.js: iOS和Android项目的统一入口文件，可以在android的MainApplication中的ReactNativeHost中重写getJSMainModuleName()方法更改; 在Ios的AppDelegate.m文件的didFinishLaunchingWithOptions方法中通过jsBundleURLForBundleRoot可以更改入口文件。

- package.json: package.json定义了项目所需要的各种模块，以及项目的配置信息（比如名称、版本、许可证等元数据）。npm install命令根据这个配置文件，自动下载所需的模块，也就是配置项目所需的运行和开发环境。

- yarn.lock: Yarn 是 一个由 Facebook 创建的新 JavaScript 包管理器；每次添加依赖或者更新包版本，yarn都会把相关版本信息写入yarn.lock文件。这样可以解决同一个项目在不同机器上环境不一致的问题。

## Hello World
![](https://oscimg.oschina.net/oscnet/d70604263d0dbf657bbb50896299eb710da.jpg)

修改App.js文件如下：
![](https://oscimg.oschina.net/oscnet/949cc57770ff780664e6781841d4beeef5c.jpg)

现在我们已经完成了Hello world， 是不是很简单呢？

## 知识点
- [JSX](https://react.docschina.org/docs/introducing-jsx.html) javascript语法糖
- [ES6](http://es6.ruanyifeng.com/)
- 组件(Component)：
上面的代码定义了一个名为HelloWorldApp的新的组件（Component）。你在编写 React Native 应用时，肯定会写出很多新的组件。而一个 App 的最终界面，其实也就是各式各样的组件的组合。组件本身结构可以非常简单——唯一必须的就是在render方法中返回一些用于渲染结构的 JSX 语句