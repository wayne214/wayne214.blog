---
title: React Native 0.61 with Fast Refresh
date: 2019-11-12 16:40:56
tags: '技术文章'
---
原文链接：[https://facebook.github.io/react-native/blog/2019/09/18/version-0.61](https://facebook.github.io/react-native/blog/2019/09/18/version-0.61)
### 快速刷新
在React Native的社区中关于最常见的痛点问题中，有一个最多的回答是“hot reloading”-热加载功能不能用了。在我们编写函数组件的时候不能正常工作，经常无法无法更新屏幕，也不能对拼写错误和错误做出正确的反映。因为这个功能不可靠，许多人都关闭了这个功能。
![在这里插入图片描述](/images/hotreload.png)
![在这里插入图片描述](/images/fastrefresh.png)
在ReactNative 0.61版本中，我们将现在的“Live Realod”和“Hot Relaoding”功能合并成了一个新功能叫“Fast Refresh”。Fast Refrsh是根据以下原则来重新实现的：

- Fast Refresh全面支持最新的React版本，包括函数组件和Hooks
- Fast Refresh在出现拼写错误和其他错误时能够正常好的恢复，并且在需要时可以回滚去重新全部加载
- Fash Refresh不会对代码进行侵入性转换，因此可以放心的设置为默认打开。

下面是一个演示视频地址：[Fast Refresh Video](https://facebook.github.io/react-native/img/homepage/ReactRefresh.mp4)

下面是一些关于Fast Refresh的小提示：

- Fast Refresh默认会在函数式组件(和 Hooks)中保留React的当前状态
- 如果你需要每次编译都重置React State,你需要在文件中添加一个特殊的 `// @refresh reset`命令到组件上。
- Fast Refresh总会在不保留状态的情况下，重新装载类组件。这样可以确保它是可靠的。
- 在代码中我们总会犯错！Fast Refresh在你保存文件的时候会自动尝试重新加载。修复拼写错误或者一个运行时报错后，你不需要手动重新加载你的APP。
- 在写代码的手添加`console.log`或者`debugger`调试语块是一个非常棒的调试技巧。

### 其他改进
- 修复了在0.60版本中使用 use_frameworks问题以及对CocoaPods的支持。
- 增加了useWindowDimensions 钩子函数，多数情况下可以替换DimensionAPI
- React 版本更新到了16.9，移除了一些不安全的生命周期函数名称。
### 重大变化
- 从0.61版本开始，移除了对React .xcodeproj的支持并删除的相关文件。


个人水平有限，难免翻译的有不正确的地方，希望大家谅解。

欢迎大家关注我的公众号：君伟说，或者添加我个人微信：wayne214。相互交流共同进步。

![个人微信公众号](/images/个人微信公众号.jpg)
