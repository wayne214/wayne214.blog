---
title: Androidgradle编译报错:Cause:org.jetbrains.plugins.gradle
date: 2019-11-06 17:56:40
tags: “Android”
---
新建了一个基于ReactNative version0.60.5的新项目，在使用Android Studio编译项目的时候build了如下错误：
```
org.jetbrains.plugins.gradle.tooling.util.ModuleComponentIdentifierImpl.getModuleIdentifier()
```
从报错类型看是gradle的版本问题，google后发现是应该是android的IDE版本(3.1.2)太低，和项目的gradle版本无法兼容导致。
项目中的版本为，通过查看/gradle/wrapper/gradle-wrapper.properties 文件中的内容，看到下面是5.4.1
```
distributionBase=GRADLE_USER_HOME
distributionPath=wrapper/dists
distributionUrl=https\://services.gradle.org/distributions/gradle-5.4.1-all.zip
zipStoreBase=GRADLE_USER_HOME
zipStorePath=wrapper/dists
```
考虑项目兼容问题，目前考虑降低gradle的版本，改下下面这样：
```
distributionBase=GRADLE_USER_HOME
distributionPath=wrapper/dists
distributionUrl=https\://services.gradle.org/distributions/gradle-4.10.1-all.zip
zipStoreBase=GRADLE_USER_HOME
zipStorePath=wrapper/dists
```
你以为这样就完事了？too young too simple.
重新build就报了如下错误：
```
Could not get unknown property 'mergeResourcesProvider' for object of type com.android.build.gradle.internal.api.ApplicationVariantImpl.
```
怎么办？接着搞呗。
进入Android根目录的build.gradle文件，修改如下：
```
dependencies {
        classpath("com.android.tools.build:gradle:3.4.1")
        // 修改上面的gradle版本成下面的：
        classpath("com.android.tools.build:gradle:3.3.0")
        // NOTE: Do not place your application dependencies here; they belong
        // in the individual module build.gradle files
    }
```
重新build项目就ok了。

欢迎关注我的公众号：君伟说。
![个人微信公众号.jpg](/images/个人微信公众号.jpg)
个人博客：https://blog.csdn.net/wayne214