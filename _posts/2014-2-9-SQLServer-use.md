---
title: 学习SQLServer中的问题解决与知识
description: 初次接触SQLServer，谈谈其用法
keywords: SQLServer
layout: post
tags: [经验]
---

####一、遇到的问题与解决####
  1、使用SQLServer是无法连接服务器
   （1）首先确定数据库名称是否正确（如果是本地数据库，数据库名一般是自己的电脑的名称，也可以用“.”代替；
   （2）SQLServer配置管理器——〉SQLServer服务——〉（启动）SQLServer代理
       （ps：注意SQLServer配置管理器中的一些网络配置以及协议要打开）
 
  2、Asp.net连接SQLServer时错误（主要原因是sql语句写错）
   （1）数据连接语句，如果数据库是express版则是：
         string connstr = @"server=lc\sqlexpress;database=newssystem;uid=sa;pwd=zcz";
        如果是企业版最好用：
         string connstr = @"server=（local）;atabase=newssystem;uid=sa;pwd=zcz";
   （2）检查SQLServer服务器是否开启
   （3）检查SQLServer配置管理器中的一些网络配置以及协议是否为启动


####二、SQLServer常用语句####
   --------创建一个包含一个主要数据文件，一个次要数据文件，一个日志文件
   create database stu1
   on primary
   -----------------默认主要数据文件要放在主要文件组
   (
   name=stu_data,
   filename='d:\stu11.mdf',------主要数据文件
   size=5,
   maxsize=100,	б
   filegrowth=1
   ),
   ------查看指定数据库
   exec sp_helpdb stu1

   ----查看数据文件的信息(在当前数据库下)
   use stu1
   exec sp_helpfile 
   exec sp_helpfile stu_data

   ------查看文件组信息
   exec sp_helpfilegroup
 
