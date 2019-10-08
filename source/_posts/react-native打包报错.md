---
title: react-native安卓打正式包报错
date: 2019-04-24 14:18:07
tags: “ReactNative”
---
笔者在工作开发任务中，最近在进行Android打release包测试时，遇到了如下报错，鼓捣了好久(甚是郁闷)，终于解决了。

### ReactNative版本环境如下
![image.png](https://upload-images.jianshu.io/upload_images/3112038-c0731c55cad9d047.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

<!-- more -->
### 问题描述
- 直接使用react-native run-android运行debug没有问题
- 在没有添加react-native-spinkit这个第三方库是打包也正常
- 添加react-native-spinkit第三库，进行run-android debug运行也正常
- 但是使用cd android && ./gradlew assembleRelease命令打正式包就build失败了
### 报错信息如下：
![image.png](https://upload-images.jianshu.io/upload_images/3112038-be00fae0b55e595c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
于是开始Google这个错误，
```
Daemon:  AAPT2 aapt2-3.2.1-4818971-osx Daemon #0
```
但是各种答案都不能解决这个问题，而且还牵涉出其他的新问题。
思来想去，应该是添加的第三库react-native-spinkit出现了问题，终于在issues中找到了答案。
原来是第三库中的buildTools,compileSdk 和targetSdk的版本和项目中的对应的版本号不一致导致的。
### 解决方案如下
在项目中android\build.gradle文件中的'allProjects'的下方添加如下代码
![image.png](https://upload-images.jianshu.io/upload_images/3112038-ac91766388109767.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```
allprojects {
    repositories {
				// Add jitpack repository (added by react-native-spinkit)
				maven { url "https://jitpack.io" }
        mavenLocal()
        google()
        jcenter()
        maven {
            // All of React Native (JS, Obj-C sources, Android binaries) is installed from npm
            url "$rootDir/../node_modules/react-native/android"
        }
    }
}
在allprojects下方添加如下代码
 subprojects {
        afterEvaluate {
            project ->
                if (project.hasProperty("android")) {
                    android {
                        compileSdkVersion = rootProject.compileSdkVersion
                        buildToolsVersion = rootProject.buildToolsVersion
                    }
                }
        }
    }
```
### 打包
添加完成后，重新使用cd android && ./gradlew assembleRelease 命令进行打包就顺利成功的打包了，成功截图如下
![image.png](https://upload-images.jianshu.io/upload_images/3112038-7db5a60dd400d7ba.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
