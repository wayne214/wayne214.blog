---
title: ReactNative文件读写操作
date: 2018-11-05 14:29:26
tags: 'ReactNative'
categories: '技术分享'
---
最近公司项目要求进行定时上传位置信息，及埋点，因为使用的是RN开发，一开始就是想到在Android和Ios原生里进行操作。
在原生里面实现了定时任务，Android里面使用的是broadcastReceive + service + timer实现了。
现在需要生成一个日志文件，一开始想在原生里面进行实现文件的读写。后来查找相关资料，发现了一个不错的第三方插件，react-native-fs，现在记录一下，集成步骤及简单的文件读写操作。
插件地址：https://github.com/itinance/react-native-fs
自己写了个Demo进行了测试，目前没有什么问题,Demo地址
https://github.com/wayne214/RNstudyDemo/blob/master/src/utils/readAndWriteFileUtil.js

<!--more -->
1.集成
安装命令：
```
npm install react-native-fs --save
```
//注意：如果react native版本是<0.40安装，使用此标签：
```
npm install react-native-fs@2.0.1-rc.2 --save
```
安装后执行：
```
react-native link react-native-fs
```
Android添加相应权限
```
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
    <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
```
2.使用
导入及设置文件存储路径

![image.png](http://upload-images.jianshu.io/upload_images/3112038-fda4523e66b1db9d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
读写操作

![image.png](http://upload-images.jianshu.io/upload_images/3112038-34dfe0b5fa0b414d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
删除文件

![image.png](http://upload-images.jianshu.io/upload_images/3112038-2b5e25ab7c8b55ec.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
获取文件路径

![image.png](http://upload-images.jianshu.io/upload_images/3112038-5fea9be6ec22fb3e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
判断文件路径是否存在

![image.png](http://upload-images.jianshu.io/upload_images/3112038-3e45ae6309cb1289.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
拷贝文件

![image.png](http://upload-images.jianshu.io/upload_images/3112038-f51d5334ef93ea74.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
创建目录
/*创建目录*/
    mkDir() {
        const options = {
            NSURLIsExcludedFromBackupKey: true, // iOS only
        };

        return RNFS.mkdir(destPath, options)
            .then((res) => {
                console.log('MKDIR success');

            }).catch((err) => {
                console.log('err', err);
            });
    }