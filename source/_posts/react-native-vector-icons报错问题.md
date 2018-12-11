---
title: react-native-vector-icons报错问题
date: 2018-11-20 11:59:35
tags: 'ReactNative'
---
最近在写项目的时候，在调试代码的过程中，偶尔会报如下错误，想必好多同学都遇到了，现在记录一下，和大家分享。
```
error: bundling failed: Error: While resolving module `react-native-vector-icons/dist/EvilIcons`, the Haste package `react-native-vector-icons` was found. However the module `dist/EvilIcons` could not be found within the package. Indeed, none of these files exist:

  * `/Users/user/Desktop/testapp/node_modules/react-native/local-cli/core/__fixtures__/files/dist/EvilIcons(.native||.android.js|.native.js|.js|.android.json|.native.json|.json)`
  * `/Users/user/Desktop/testapp/node_modules/react-native/local-cli/core/__fixtures__/files/dist/EvilIcons/index(.native||.android.js|.native.js|.js|.android.json|.native.json|.json)`

```
解决问题第一大法，去github上找到这个项目的issues中，在
https://github.com/oblador/react-native-vector-icons/issues/626中看到了解决方案：
在项目的根目录下，执行一下如下代码就OK了
```
rm ./node_modules/react-native/local-cli/core/__fixtures__/files/package.json
```