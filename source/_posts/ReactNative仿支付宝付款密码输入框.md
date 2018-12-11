---
title: ReactNative仿支付宝付款密码输入框
type: 'ReactNative'
---

接触RN差不多三个月了，发布一个仿支付宝付款密码的输入框控件，一直都是在使用大神们发布的控件，收益良多，现在发布第一个控件，也是基于一位大神写的控件，站在巨人肩膀上，在此感谢，文中有大神封装的链接，大家可以看看，学习npm发布的步骤，也增加了自己学习的自信心和动力。以后工作中会逐渐发布一些自己写的库，还忘朋友们多多支持。
<!--more-->
react-native-password-pay
采用React Native开发，仿支付宝付款密码输入框,借鉴了大神封装的类库，在此表示感谢[https://github.com/chenchunyong/react-native-passwordInput，采用ES6语法进行改写](https://github.com/chenchunyong/react-native-passwordInput%EF%BC%8C%E9%87%87%E7%94%A8ES6%E8%AF%AD%E6%B3%95%E8%BF%9B%E8%A1%8C%E6%94%B9%E5%86%99)
[![仿支付宝密码输入框](https://github.com/wayne214/react-native-password-pay/raw/master/password.png)](https://github.com/wayne214/react-native-password-pay/raw/master/password.png)
Install
安装包:
npm i react-native-password-pay --save

通过引用import Password from 'react-native-password-pay'
来使用
Demo
```
<View> 
<Password maxLength={6}></Password> 
</View>
```
其中maxLength={6}
表示需要输入6位数密码
