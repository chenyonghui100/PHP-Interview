# PHP知识点总结
---
## 1.计算机网络体系结构
   * [计算机网络体系结构](./docs/01网络/QA.md#体系概述)
   * [一次网络请求的完整生命周期](./docs/01网络/QA.md#请求完整生命周期)
   * [TCP,UDP主要特点](./docs/01网络/QA.md#TCP_UDP主要特点)
   * [TCP,UDP区别以及适用场景](./docs/01网络/QA.md#TCP_UDP区别)
   * [三次握手和四次挥手](./docs/01网络/QA.md#三次握手四次挥手)
   * [建立TCP为什么要第三次握手](./docs/01网络/QA.md#三次握手原因)
   * [TIME-WAIT是什么？为什么必须等待2MLS](./docs/01网络/QA.md#TIME-WAIT作用)
   * [DNS主要作用](./docs/01网络/QA.md#DNS作用)
   * [GET与POST请求的区别](./docs/01网络/QA.md#GET与POST区别)
   * [HTTP报文组成](./docs/01网络/QA.md#HTTP报文组成)
   * [HTTP状态码](./docs/01网络/QA.md#HTTP状态码)
   * [HTTPS与HTTP区别](./docs/01网络/QA.md#HTTPS与HTTP区别)
   * [什么是长连接](./docs/01网络/QA.md#长连接)
   * socket

## 2.数据结构与算法
   * [衡量算法优劣的指标](./docs/02数据结构与算法/QA.md#算法优劣指标)
   * [数组](./docs/02数据结构与算法/QA.md#数组)
   * 链表（[单向链表](./docs/02数据结构与算法/QA.md#单向链表)，[循环链表](./docs/02数据结构与算法/QA.md#循环链表)，[双向链表](./docs/02数据结构与算法/QA.md#双向链表)）
   * [特殊的线性表：栈](./docs/02数据结构与算法/QA.md#栈)
   * [特殊的线性表：队列 ](./docs/02数据结构与算法/QA.md#队列)
   * 二叉树（[二叉查找树](./docs/02数据结构与算法/QA.md#二叉查找树)，[红黑树](./docs/02数据结构与算法/QA.md#红黑树)，[满二叉树](./docs/02数据结构与算法/QA.md#满二叉树)，[完全二叉树](./docs/02数据结构与算法/QA.md#完全二叉树)）
   * [二分查找](./docs/02数据结构与算法/QA.md#二分查找)
   * [冒泡排序](./docs/02数据结构与算法/QA.md#冒泡排序)
   * [插入排序](./docs/02数据结构与算法/QA.md#插入排序)
   * [选择排序](./docs/02数据结构与算法/QA.md#选择排序)
   * [归并排序](./docs/02数据结构与算法/QA.md#归并排序)
   * [快速排序](./docs/02数据结构与算法/QA.md#快速排序)
## 3.PHP
  * [概念理解](./docs/03php基础/QA.md#概念理解)
  * [八大数据类型](./docs/03php基础/QA.md#快速排序)
  * [echo,print,print_r,var_dump区别](./docs/03php基础/QA.md#八大数据类型)
  * [isset 和empty 区别](./docs/03php基础/QA.md#判空函数区别)
  * [include ,require ,include_once ,require_once 区别](./docs/03php基础/QA.md#包含函数区别)
  * [常用的数组处理函数](./docs/03php基础/QA.md#array函数)
  * [cookie 和session 的区别](./docs/03php基础/QA.md#cookie与session)
  * [构造函数和析构函数](./docs/03php基础/QA.md#构造析构函数)
  * [魔术方法](./docs/03php基础/QA.md#)
  * [类中修饰符public ,private ,protected 表示什么](./docs/03php基础/QA.md#访问修饰符)
  * [类的静态调用和实例化调用](./docs/03php基础/QA.md#类调用)
  * [面向对象理解](./docs/03php基础/QA.md#面向对象)
  * [Composer](./docs/03php基础/QA.md#composer)
## 4.WEB前端
  * [web 语义化](./docs/04web前端/QA.md#Web语义化)
  * [CSS选择器分类](./docs/04web前端/QA.md#CSS选择器)
  * [为什么js文件要放在文件底部](./docs/04web前端/QA.md#js文件问什么放在底部)
  * [js操作函数](./docs/04web前端/QA.md#js函数)
  * [如何解决跨域问题](./docs/04web前端/QA.md#跨域问题)
  * [同源策略是什么](./docs/04web前端/QA.md#同源策略)
  * [setTimeout 和 setInterval 的区别](./docs/04web前端/QA.md#定时函数)
  * [懒加载](./docs/04web前端/QA.md#懒加载)

## 5.MySQL
 * [体系结构](./docs/05mysql/QA.md#体系结构)
 * [SQL语句怎么执行的](./docs/05mysql/QA.md#SQL语句执行)
 * [基础操作语句](./docs/05mysql/QA.md#基础操作语句)
 * [数据库设计原则](./docs/05mysql/QA.md#设计原则)
 * [CHAR 和VARCHAR 区别](./docs/05mysql/QA.md#字符型区别)
 * [LEFT JOIN,RIGHT JOIN,INNER JOIN](./docs/05mysql/QA.md#连接查询)
 * [常用的MySQL函数](./docs/05mysql/QA.md#MySQL函数)
 * [锁](./docs/05mysql/QA.md#锁)
 * [事物](./docs/05mysql/QA.md#事物)
 * [MYISAM和INNODB](./docs/05mysql/QA.md#引擎)
 * [常见索引](./docs/05mysql/QA.md#常见索引)
 * [BTree+](./docs/05mysql/QA.md#BTree+)
## 6.Redis
  * [Redis 特点](./docs/06redis/QA.md#Redis)
  * [Redis 为什么这么快](./docs/06redis/QA.md#Redis为什么这么快)
  * [支持的数据结构及命令](./docs/06redis/QA.md#redis命令)
  * [持久化策略](./docs/06redis/QA.md#持久化策略)
  * [Redis事物](./docs/06redis/QA.md#Redis事物)
  * [单线程如何应对并发请求](./docs/06redis/QA.md#单线程如何应对并发请求])
  * [Redis 过期策略及内存淘汰机制](./docs/06redis/QA.md#内存淘汰机制)

## 7.Linux
  * [Linux 基础命令](./docs/07linux和git/QA.md#基础命令)
  * [文件查找](./docs/07linux和git/QA.md#文件查找)
  * [权限命令](./docs/07linux和git/QA.md#权限命令)
  * [crontab 定时任务](./docs/07linux和git/QA.md#crontab)
  * [Vim](./docs/07linux和git/QA.md#Vim)
  * shell编程
  * [Git](./docs/07linux和git/QA.md#git)

## 8.网络安全
  * [跨站脚本攻击(XSS)](./docs/08web安全/QA.md#XSS)
  * [跨站点请求伪造(CSRF)](./docs/08web安全/QA.md#CSRF)
  * [SQL 注入](./docs/08web安全/QA.md#SQL注入)
  * [刷新提交](./docs/08web安全/QA.md#刷新提交)
  * [漏洞扫描](./docs/08web安全/QA.md#漏洞扫描)
  * [DDOS攻击](./docs/08web安全/QA.md#DDOS攻击)


## 9.设计模式
  * [如何理解设计模式](./docs/09设计模式/QA.md#设计模式概述)
  * [单例模式](./docs/09设计模式/QA.md#单例模式)
  * [简单工厂模式](./docs/09设计模式/QA.md#工厂模式)
  * [控制反转与依赖注入](./docs/09设计模式/QA.md#IOC和DI)
  * [观察者模式](./docs/09设计模式/QA.md#观察者模式)
  * 待完善

## 10.框架篇（Laravel）
   * Laravel
     - [生命周期](./docs/10框架篇/QA.md#生命周期)
     - [路由(Web,Api)](./docs/10框架篇/QA.md#路由)
     - [中间件](./docs/10框架篇/QA.md#中间件)
     - [ORM](./docs/10框架篇/QA.md#ORM)
     - [邮件发送](./docs/10框架篇/QA.md#Mail)
     - [缓存](./docs/10框架篇/QA.md#Cache)
   * YII
     
     - [模型相关](./docs/10框架篇/Yii.md#_6)
     
     - - [获取查询SQL](./docs/10框架篇/Yii.md#SQL_8)
       - [查询数据表中具体列](./docs/10框架篇/Yii.md#_16)
       - [`Expression()`函数](./docs/10框架篇/Yii.md#Expression_24)
       - [模型中事务编写](./docs/10框架篇/Yii.md#_44)
       - [Model 里 rules 联合唯一规则](./docs/10框架篇/Yii.md#Model__rules__66)
       - [是否存在的规则](./docs/10框架篇/Yii.md#_72)
       - [表单验证两个字段选取一个](./docs/10框架篇/Yii.md#_83)
       - [Like 模糊查询](./docs/10框架篇/Yii.md#Like__97)
       - [执行SQL查询并缓存结果](./docs/10框架篇/Yii.md#SQL_102)
       - [`and`和`or`共用](./docs/10框架篇/Yii.md#andor_112)
       - [嵌套查询](./docs/10框架篇/Yii.md#_123)
     
     - [控制器相关](./docs/10框架篇/Yii.md#_136)
     
     - - [获取模块/控制器/动作的id](./docs/10框架篇/Yii.md#id_138)
       - [表单提交失败调试](./docs/10框架篇/Yii.md#_147)
       - [文件下载](./docs/10框架篇/Yii.md#_152)
       - [打印数据](./docs/10框架篇/Yii.md#_168)
       - [控制器调用其他控制器方法](./docs/10框架篇/Yii.md#_175)
       - [获取GET数据](./docs/10框架篇/Yii.md#GET_182)
     
     - [视图相关](./docs/10框架篇/Yii.md#_191)
     
     - - [视图中获取当前模块/控制器/方法id](./docs/10框架篇/Yii.md#id_193)
     
     - [防止SQL注入或者XSS攻击](./docs/10框架篇/Yii.md#SQLXSS_202)
     
     - [配置相关](./docs/10框架篇/Yii.md#_211)
     
     - - [用户组件登录修改](./docs/10框架篇/Yii.md#_212)
       - [配置文件IP白名单](./docs/10框架篇/Yii.md#IP_224)
     
     - [队列](./docs/10框架篇/QB.md#queue)
     
     - 
   * [接口请求频率限制](./docs/10框架篇/QC.md#接口请求频率限制)
   * [接口返回数据](./docs/10框架篇/QC.md#接口返回数据)
   * [CURL请求接口](./docs/10框架篇/QC.md#curl)
## 11.架构篇
  * [如何让系统支持高并发](./docs/11架构篇/QA.md#如何让系统支持高并发)
  * Nginx配置([正向代理](./docs/11架构篇/QA.md#正向代理),[反向代理](./docs/11架构篇/QA.md#反向代理),[负载均衡](./docs/11架构篇/QA.md#负载均衡))
  * [单点登录](./docs/11架构篇/QA.md#单点登录)
  * [REST 规范](./docs/11架构篇/QA.md#REST规范)
  * [缓存雪崩](./docs/11架构篇/QA.md#缓存雪崩),[缓存穿透](./docs/11架构篇/QA.md#缓存穿透)
  * [如何保证缓存与数据库的一致性](./docs/11架构篇/QA.md#缓存与数据库的一致性)
  * [ElasticeSearch(分布式架构索引)](./docs/11架构篇/QA.md#ElasticeSearch)
  * [Kafka](./docs/11架构篇/QA.md#Kafka)
  * docker
  * swoole
  * mycat







