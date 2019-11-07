---
title: 来了，来了，真的来了，ReactNative集成百度语音开源库
date: 2019-10-22 15:05:14
tags: 'react-native-baidu-vtts'
categories: '开源软件'
---
笔者在之前这篇文章中[ReactNative集成百度语音合成](https://www.jianshu.com/p/232b860676ab)介绍了在项目中如何集成百度语音合成步骤和部分代码。
今天重(you)磅(dian)推(peng)出(zhang)百度语音合成开源库# **[react-native-baidu-vtts](https://github.com/wayne214/react-native-baidu-vtts)** ,目前只做了Android端的集成，后期补上IOS端集成，开源不易，欢迎大家star
## Github地址：**[react-native-baidu-vtts](https://github.com/wayne214/react-native-baidu-vtts)**

### 开源的初衷：
* 虽然能够在项目中集成，但是使用起来还是需要写Android类和js代码，相对有些繁琐
* 部分读者在上一篇文章反馈，按照笔者的步骤集成中出现一些问题，比较闹心
* 网上也有一些类似的开源库，存在如下一下问题：没有使用文档、根本就不是一个库、兼容性问题(Android机型)[ReactNative集成百度语音合成](https://www.jianshu.com/p/232b860676ab)文章有也有提到。
* 一直想开源。。。
### 准备工作
请参考[ReactNative集成百度语音合成](https://www.jianshu.com/p/232b860676ab)的“兵马未动粮草先行”模块
### 安装
```
npm install react-native-baidu-vtts --save
or
yarn add react-native-baidu-vtts 
自动添加原生依赖
react-native link react-native-baidu-vtts
```
### 使用
```
import RNBaiduvoice from 'react-native-baidu-vtts';

// TODO: What to do with the module?
class App extends Component{

    componentDidMount() {
    	// 填写百度语音官网申请的appid, apikey, secretkey
    	const String appid = ''
    	const String apikey = ''
    	const String secretkey = ''
        RNBaiduvoice.initBaiduTTS(appid,apikey,secretkey)
    }

    _speechText = () => {
        RNBaiduvoice.speak('百度语音')
    }

    render() {
        return (
            <View style={styles.container}>
                {/*<TwoList/>*/}
                <TouchableOpacity onPress={this._speechText}>
                    <Text style={{fontSize: 20, height: 30}}>测试语音</Text>
                </TouchableOpacity>
            </View>
        );
    }
}
```
### 最后
欢迎关注我的公众号：君伟说， 一个有温度的公众号。
![个人微信公众号.jpg](/images/个人微信公众号.jpg)