---
title: Android之Activity生命周期
date: 2018-11-05 14:51:51
tags: 'Android'
---
官方Activity生命周期图例：
![060009291302389.png](https://upload-images.jianshu.io/upload_images/3112038-c2e46272bcbd5727.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

知识点回顾：
1.onStart和onResume,onPause和noStop，从功能描述上差不多，有什么区别？
onStart和onStop是从Activity是否可见这个角度回调的，而onResume和onPause是从Activity是否位于前台这个角度来回调的。
2.假设当前Activity为A ，如果此时打开了一个新的ActivityB,那么是B的onResume和A的onPause那个先执行？
答： 是旧的的Activity先onPause, 然后新Activity再启动。
3.当使用ApplicationContext启动standard模式的activity时会报错
原因：因为standard模式的Activity会默认进入启动它的Activity任务栈中，但是非Activity类型的Context并没有所谓的任务栈。
解决方案：对待启动的activity指定FLAG_ACTIVITY_NEW_TASK标记位，这样启动的时候就会为它创建一个新的任务栈，这时候待启动的Activity实际上是以singleTask模式启动的。

Tips:  
* 系统只在Activity异常终止的时候才会调用onSaveInstanceState和onRestoreInstanceState来存错和恢复数据，其他情况不会触发这个过程。
* 如果设置了Activity的configeChanges属性，例如
android:configChanges="orientation|locale|keyboardHidden",在横竖屏切换的时候，不会重新创建Activity，也就不会重新走Activity的生命周期。
