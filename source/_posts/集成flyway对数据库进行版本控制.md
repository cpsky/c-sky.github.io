---
title: 集成flyway对数据库进行版本控制
date: 2019-06-20 22:21:19
categories: 框架
tags: [flyway]
---

## 前言

​	[flyway官网](https://flywaydb.org/) 是这样描述自己的产品的：flyway是数据库的版本控制工具，它可以使数据库迁移变得简单。对我这种个人学习者的人来说，第二点太重要了，如果我换了个电脑，在github上clone下来源代码，通过执行一条命令，数据库就全部建好了。Springboot如何集成flyway呢？如何使用呢？

## 步骤

### 1.在pom文件中添加plugin

​	示例如下：

```xml
          <plugin>
                <groupId>org.flywaydb</groupId>
                <artifactId>flyway-maven-plugin</artifactId>
                <version>5.2.4</version>
                <configuration>
                    <url>jdbc:mysql://localhost/community?serverTimezone=GMT</url>
                    <user>root</user>
                    <password>*****</password>
                </configuration>
                <dependencies>
                    <dependency>
                        <groupId>mysql</groupId>
                        <artifactId>mysql-connector-java</artifactId>
                        <version>8.0.16</version>
                    </dependency>
                </dependencies>
            </plugin>
```

​	根据自身情况对相关联数据库进行配置。

### 2.创建Migration文件

![](http://ptab4lsol.bkt.clouddn.com/flywaystep1.jpg)

 ![](http://ptab4lsol.bkt.clouddn.com/flywaystep2.jpg)

​	步骤很简单，唯一需要注意的点是：文件名必须是Vn(n递增)__(两个下划线)\*.sql      \*里面就是一些描述性语言

文件里面的内容就是你对数据库做出的修改。

### 3.在终端执行命令

> mvn flyway:migrate

​	万事ok!