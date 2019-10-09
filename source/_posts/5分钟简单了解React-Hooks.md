---
title: 5分钟简单了解React-Hooks
date: 2019-10-09 18:44:20
tags: 'react-hooks'
---
首先附上官网正文😀：[React Hooks]([https://reactjs.org/docs/hooks-intro.html](https://reactjs.org/docs/hooks-intro.html)
)
> Hooks are a new addition in React 16.8. They let you use state and other React features without writing a class.

简单讲就是说，Hooks是在React 16.8版本中新增加的特性。可以让你通过不谢一个类class,就可以使用State和其他React特性。
另外使用ReactNative开发的小伙伴需要注意一下，就是在ReactNative 0.59正式版才开始支持Hooks.
> React 16.8.0 is the first release to support Hooks. When upgrading, don’t forget to update all packages, including React DOM. React Native supports Hooks since [the 0.59 release of React Native](https://facebook.github.io/react-native/blog/2019/03/12/releasing-react-native-059).

现在通过写一个简单的案例来说明一下，下面代码中useState方法就是React已经内部实现的hook,是一个状态钩子。
#### 1.使用class的方式,通过一个按钮点击改变文字
```
import React, {Component} from 'react'
import {
    Text,
    View,
    TouchableOpacity
} from 'react-native'

export default class demo_class extends Component {
    constructor(props) {
        super(props)
        this.state = {
            text: 'this is class demo'
        }
    }
    _changeTheDefaultText = () => {
        this.setState({
            text: 'this is the new text'
        })
    }
    render() {
        const {text} = this.state
        return (
            <View>
                <Text style={{fontSize: 20, color: 'red'}}>{text}</Text>
                <TouchableOpacity onPress={this._changeTheDefaultText}>
                    <Text style={{fontSize: 20, color: 'red'}}>改变文字</Text>
                </TouchableOpacity>
            </View>
        )
    }
}
```
#### 2.同样的方式，使用hook来实现
```
import React, {useState} from 'react'
import {
    Text,
    View,
    TouchableOpacity
} from 'react-native'

const demoHooks = () => {
    // 初始值
    const [text, setText] = useState('this is hook demo')
    // 方法
    _changeTheDefaultText = () => {
        return setText('this is the new text')
    }
    return (
        <View>
            <Text style={{fontSize: 20, color: 'red'}}>{text}</Text>
            <TouchableOpacity onPress={this._changeTheDefaultText}>
                <Text style={{fontSize: 20, color: 'red'}}>改变文字</Text>
            </TouchableOpacity>
        </View>
    )
}

export default demoHooks
```
通过上面两个简单的例子，直观看使用hook后的代码数明显比使用class完成的少，但实现的是同样的功能。是不是很欣喜呢？
> useState()这个函数接受一个初始值作为参数，和class中this.state设置的值一样。返回一个数组，数组的第一个成员是我们自定义的变量，就是状态的当前值，比如，例子中text;第二个成员是一个函数，用来修改state,通常约定set前缀加上自定义状态的变量名，比如例子中的setText
#### 3.新增加的这个hooks特性，没有突破性的改变
- 是一个可选项。你完全可以尝试使用Hooks重写任何已有代码
- 100%向后兼容。Hooks并没有带来任何突破性的改变。
- hooks只是提供了一个多选项，一个more API,来优化你的代码， react 官方也没有打算要完全移除类class