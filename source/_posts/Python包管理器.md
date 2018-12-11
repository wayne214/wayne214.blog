---
title: Python包管理器-pip
date: 2018-11-20 11:41:43
tags: 'Python'
---
pip 是 Python 包管理工具，该工具提供了对Python 包的查找、下载、安装、卸载的功能。

目前如果你在 [python.org](https://www.python.org/) 下载最新版本的安装包，则是已经自带了该工具。

Python 2.7.9 + 或 Python 3.4+ 以上版本都自带 pip 工具。

pip 官网：[https://pypi.org/project/pip/](https://pypi.org/project/pip/)

<!-- more -->

你可以通过以下命令来判断是否已安装：
```
pip --version
```
如果没有安装，可使用如下命令进行安装：
```
curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
python get-pip.py
```
注意：
>注意：用哪个版本的 Python 运行安装脚本，pip 就被关联到哪个版本，如果是 Python3 则执行以下命令：
$ sudo python3 get-pip.py    # 运行安装脚本。
一般情况 pip 对应的是 Python 2.7，pip3 对应的是 Python 3.x。

##pip常用命令
获取帮助
```
pip --help
```
查看版本及路径
```
pip --version
```
升级pip的版本
```
pip install --upgrade pip
```
安装包
```
pip install SomePackage              # 最新版本
pip install SomePackage==1.0.4       # 指定版本
pip install 'SomePackage>=1.0.4'     # 最小版本
```
升级包
```
pip install --upgrade SomePackage
```
卸载包
```
pip uninstall SomePackage
```
参看安装包信息
```
pip show 
```
查看指定安装包信息
```
pip show -f SomePackage
```
搜索包
```
pip search SomePackage
```
列出已安装的包
```
pip list
```
查看可升级的包
```
pip list -o
```