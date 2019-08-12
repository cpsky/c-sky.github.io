---
title: springboot练手项目遇到的那些坑
date: 2019-07-04 22:37:12
categories: 工程
tags: [springboot,spring,mybatis]
---

## 前言

​	上个月跟着视频做了一个社区项目，学习了spring、springboot、mybatis的用法，框架的使用方法还是蛮好理解的，在这过程中掌握的知识就单独写几篇博客来说明吧，这篇博文主要讲在这个项目中遇到的那些坑 

## 坑

1. 使用的mysql数据库，并且将某些字段设置了默认值，但是通过Mybatis框架插入的时候，还是插入了null值，原来mybatis的insert方法是将所有字段强行全部插入。**解决办法**：在插入之前给对象赋初始值。

2. 由于在github上的项目不能暴露给别人使用云的key和id，所以我们在application.properties的同级目录下copy一个新的文件叫**application-production.properties**在里面放置我们生产环境中使用的key和id。此文件只存在于生产环境上，不提交git

3. 我买了阿里云的域名，将域名解析到云主机上，结果竟然没Ping通，找了很多方法也没用，结果过了20分钟自己通了。绑定域名并不复杂，只需要在域名控制台进行dns解析到云主机的弹性ip就可以了

4. 因为项目调用了githun的授权登录接口，所以通过域名访问失败，github开发者应用中修改如下设置就可以了

   ![](https://cxlsky.oss-cn-beijing.aliyuncs.com/blog/img/githubauthorize.jpg?x-oss-process=style/blogimg)

## 部署

​	我选择的服务器是centos7，记录一下服务器环境搭建的步骤

要安装的环境：

- git
- jdk
- maven
- mysql

部署步骤：

1. **yum update**: 更新安装工具
2. **yum install git**
3. **git clone (源码地址)**
4. **yum install maven**:自动会装好jdk
6. [参考的博客](<https://blog.csdn.net/baidu_32872293/article/details/80557668>)      重启Mysql:`service mysqld restart`  查看mysql是否启动·`service mysqld status`
7. Mysql环境安装好之后用navicat无论如何也连接不上，该授权的授权了，防火墙也都关闭了，查询原因我用的是ucloud的云主机，需要在ucloud控制台中把3306端口开放。
8. 创建数据库，之后在community目录下运行**mvn flyway:migrate**，集成flyway我之前做过总结。
8. **mvn clean compile package**:项目打包，在项目目录下执行
9. **java -jar -Dspring.profiles.active=production target/community-0.0.1-SNAPSHOT.jar** 发布项目
10. 注意mysql数据库大小写敏感问题，我修改成太小写不敏感，不然报错
11. `nohup java -jar -Dspring.profiles.active=production target/community-0.0.1-SNAPSHOT.jar &`:后台运行 不挂进程
