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

