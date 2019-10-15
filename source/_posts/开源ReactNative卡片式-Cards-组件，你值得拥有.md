---
title: 开源ReactNative卡片式(Cards)组件，你值得拥有
date: 2019-10-15 16:16:06
tags: "开源软件"
---
开源一个跨端的卡片式设计(Cards)的组件,在Android中是Material Design中有一种很个性的设计概念，在使用React-Native跨平台的开发框架中，卡片样式在IOS平台通过设置View的样式就可以实现类似的效果,比如这样：
```
<View style={{
        shadowOffset: { // 设置阴影偏移量
            width: 0,
            height: 4
        },
        shadowRadius: 4, // 设置阴影模糊半径
        shadowOpacity: 0.13, // 设置阴影的不透明度
        borderRadius: 10, // 设置圆角
        shadowColor: 'rgba(96,96,96,1)' // 设置阴影色
        }
        {...props}
    />
```
基于此，此开源组件，在IOS端即采用了RN平台提供的阴影样式属性来实现卡片样式；在Android端采用Android原生support库在V7引入的原生CardView UI组件，来实现卡片样式设计的组件​。
Github项目地址： **[react-native-cardview-wayne](https://github.com/wayne214/react-native-cardview-wayne)**

### 使用​：
```
import RNCardview from 'react-native-cardview-wayne';

export default class App extends Component {
    render() {
        return (
				<CardView cardElevation={4}
                          maxCardElevation={4}
                          radius={10}
                          backgroundColor={'#ffffff'}>
                    <View style={{padding:10}}>
                        <View>
                            <Text>CardView for iOS and Android</Text>
                        </View>
                        <View>
                            <Text>This is test</Text>
                        </View>
                    </View>
                </CardView>
        );
    }
};
```
### 效果如下：

Android：

![image](/images/android.png)

ios:

![image](/images/ios.png)

此次重造一个轮子，目的在于学习封装一个包含原生组件的ReactNative包的开发过程，并发布到npm仓库。

个人公众号：君伟说， 欢迎大家关注。
![个人微信公众号.jpg](/images/个人微信公众号.jpg)
