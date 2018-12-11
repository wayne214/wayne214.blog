---
title: 跨平台的Webview支持onShouldStartLoadWithRequest
date: 2018-11-02 12:08:36
tags: 'ReactNative'
categories: '技术分享'
---
众所周知，React-Native原生的Webview的属性onShouldStartLoadWithRequest仅支持IOS平台，但是往往在开发中两端都需要进行URl的拦截，进行对应的业务需要。
![webview.png](https://upload-images.jianshu.io/upload_images/3112038-dfb7c659888eb02d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
通过不断学习，在https://github.com/react-native-community/react-native-webview 的基础上，整理出来一个方案，并且发布到了npm上，小伙伴们有需要的可以参考一下。https://github.com/wayne214/react-native-webview-crossplatform, 欢迎star,fork.
<!--more-->
使用方式如下：
1.添加依赖包
> yarn add react-native-webview-crossplatform
> react-native link react-native-webview-crossplatform

2.在需要的业务页面导入
```
import { WebView } from 'react-native-webview-crossplatform'
export default class webview extends Component<Props> {
    constructor(props){
        super(props);
    }


  render() {
    return (
      <View style={styles.container}>
          <WebView
              source={{ uri: 'https://infinite.red/react-native' }}
              style={{ marginTop: 20 }}
              onLoadProgress={e=>console.log(e.nativeEvent.progress)}
              onShouldStartLoadWithRequest={(e)=> {
                  console.log('拦截', e)
                  return true
              }}
              renderError={()=> {
                  return <View>
                      <Text>我是错误页面</Text>
                  </View>
              }}
          />
      </View>
    );
  }
}
```