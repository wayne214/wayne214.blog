---
title: SpringBoot入门系列HelloWorld
date: 2019-02-25 16:11:03
tags: "SpringBoot"
---
根据咱们程序员学习的惯例，学习一门新技术都是从HelloWorld开始的。
感觉编程是一件非常富有意义的事情，程序员也是一群可爱的人，渴望被关怀和关注，因为我们总在和世界say Hi.
好了进入正题
## 创建项目
首先创建一个项目，可看我上一篇文章写得
[IntelliJ IDEA创建第一个Spring boot项目](https://www.jianshu.com/p/1b944060ee68)
接下来运行这个项目，你将会看到如下页面
![image.png](https://upload-images.jianshu.io/upload_images/3112038-a7ff20ce88c3a9c8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
提示我们当前没有准确的映射，所以找不到对应的页面也就是404。莫慌，接下来咱们处理一下
## 创建HelloController控制器
在项目名/src/main/java/包名下，新建一个config包，包下面创建HelloController
```
@Controller
public class HelloController {
    @RequestMapping(value = "/hello",method = RequestMethod.GET)
    @ResponseBody
    public String hello(){
        return "Hello World";
    }
}
注解说明：
@Controller: 可让项目扫描自动检测到这个类,处理http请求
@ RequestMapping 请求的路由映射，访问的路径就是：http://localhost:8080/hello
value: 路由名
method: 请求方式，GET,POST,PUT,DELETE等
```
## 重新启动项目
```
访问：http://localhost:8080/hello, 就看到Hello World了
```
![image.png](https://upload-images.jianshu.io/upload_images/3112038-0d47fa953327cee8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
看到如上图所示，就表示我们的hello world成功了。

## 目录结构：
![image.png](https://upload-images.jianshu.io/upload_images/3112038-f286882f2b5ad6c5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
- src/main/java:	Java代码的目录
- src/main/resources: 资源目录
- src/test/java: 测试代码的目录
- src/test/resources: 测试资源目录


<!-- more -->
## 文件说明
### pom.xml文件
父项目
```
<parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.1.3.RELEASE</version>
        <relativePath/> <!-- lookup parent from repository -->
</parent>
管理Spring Boot应用里面所依赖的版本
```
管理依赖
```
<dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>

Spring Boot将所有的功能场景都抽取出来，做成一个个的starters（启动器），只需要在项目里面引入这些starter相关场景的所有依赖都会导入进来，要用什么功能就导入什么场景的启动器
```
### 主程序类，入口类
![image.png](https://upload-images.jianshu.io/upload_images/3112038-55bdc5eaab8d1025.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
@SpringBootApplication : Spring Boot应用标注在某个类上说明这个类是SpringBoot的主配置类，SpringBoot就应该运行这个类的main方法来启动SpringBoot应用；