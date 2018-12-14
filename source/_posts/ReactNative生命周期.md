---
title: ReactNative生命周期
date: 2018-12-14 11:11:06
tags: "ReactNative"
---
![ReactNative生命周期](https://upload-images.jianshu.io/upload_images/3112038-077ae05e77f257dd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
<!-- more -->
## 1.组件实例化阶段
### defaultProps:
设置组件的初始属性值，比如设置默认Color,width等，可以在通过this.props获取相应的值
### constructor(props):
这里通过this.props可以获取defaultProps设置的默认属性值，同时这里用于初始化控件的可变化的变量，通过this.state设置变量的初始值，通过this.setState()函数修改变量的值，调用render()函数重新渲染页面，得到新的页面
![image.png](https://upload-images.jianshu.io/upload_images/3112038-29975d061955c4f8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### componentWillMount：
 组件将要被加载到视图之前调用
### render(): 第一次被调用，用于渲染页面
### componentDidMount：
在调用了render方法，组件加载完成并被成功渲染出来之后，所要执行的后续操作，一般都会在这个函数中进行，比如经常要面对的网络请求等加载数据操作，因为UI渲染是异步的，所以在这个函数里面进行网络请求，能够避免出现UI错误。

## 2.组件运行时阶段
组件的属性prop和状态state任何一个改变都可能会触发render()函数渲染页面
### componentWillReceiveProps:
指父元素对组件的props进行了修改
### shouldComponentUpdate
一般用于优化性能，通过业务逻辑判断返回true或false，来决定页面是否进行重新绘制，默认返回true,执行后面两个周期函数
### componentWillUpdate:
组件刷新前调用
### componentDidUpdate：更新后
![image.png](https://upload-images.jianshu.io/upload_images/3112038-abde4cadf5964ae2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 3.页面卸载页面：
### componentWillUnmount
一般用于清理工作，比如移除事件监听，取消定时器等
![image.png](https://upload-images.jianshu.io/upload_images/3112038-dc62024c762a94d8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 4.生命周期函数调用次数
![image.png](https://upload-images.jianshu.io/upload_images/3112038-0cb98c92e14626c4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 特别提示：
更新state必须使用setState()函数，setState是一个异步的函数：setState(update,[callback])
setState()不是立刻更新组件。其可能是批处理或推迟更新。这使得在调用setState()后立刻读取this.state的一个潜在陷阱。代替地，使用componentDidUpdate或一个setState回调（setState(updater, callback)），当中的每个方法都会保证在更新被应用之后触发。

参考文档：https://react.docschina.org/docs/react-component.html