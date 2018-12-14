---
title: MacOS下安装MongoDB数据库
date: 2018-12-11 15:35:28
tags: "数据库"
---
官方链接：[Install MongoDB Community Edition on macOS](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-os-x/)
推荐大家使用[Homebrew](https://brew.sh/index_zh-tw.html)安装
##1.更新 Homebrew’s 包版本
```
brew update
```
##2.安装MongoDB
```
brew install mongodb
```
休息片刻，等他安装完就好了
默认安装在/usr/local/Cellar/mongodb/4.0.4_1（我的版本）目录下
安装好了，还需要配置一下，否则是无法正常启动服务的

<!-- more -->
##3.配置mongodb
####a.创建一个db目录，用于mongodb写数据
```
mkdir -p /data/db
```
如果出现 permission denied ，加上 sudo 命令：
```
sudo mkdir -p /data/db
```
####b.给 /data/db 文件夹赋予权限
```
sudo chown id -u /data/db
如果出现 "illegal user name" 的错误提示，这时我们可以查看当前的 username 并赋予权限：
$ whoami
username
$ sudo chown username /data/db
```
####c.配置mongodb环境变量
1.打开.zshrc 文件
```
vim ~/.zshrc
添加mongodb的安装目标到path中
export PATH=/usr/local/Cellar/mongodb/4.0.4_1/bin:${PATH}
使配置生效
source ~/.zshrc
```
2.修改 MongoDB 配置文件, 配置文件默认在 /usr/local/etc 下的 mongod.conf：
```
# Store data in /usr/local/var/mongodb instead of the default /data/db
dbpath = /data/db
# Append logs to /usr/local/var/log/mongodb/mongo.log
logpath = /usr/local/var/log/mongodb/mongo.log
logappend = true


# Only accept local connections
bind_ip = 127.0.0.1
```
3.启动mongod服务
```
mongod
```
![image.png](https://upload-images.jianshu.io/upload_images/3112038-bc82d65c982c369d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
当出现上图红框中的命令时，就表明服务启动成功了，侦听端口为27017，这是mongod的默认端口，在另外的一个窗口中使用mongo就可以打开客户端：
```
mongo
```
![image.png](https://upload-images.jianshu.io/upload_images/3112038-5227876fc02d2d98.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这时候就可输入数据库命令进行操作了，比如show dbs,可以查看当前的数据库集合了
![image.png](https://upload-images.jianshu.io/upload_images/3112038-8783d295aa4c6f79.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我的网站：https://wayne214.github.io