---
title: js引擎
date: 2017-3-20 14:07:43
tags: js高级
categories: js高级
---

## 定时器 / JS线程 / 事件循环模型问题
 * 定时器并不能真正保证定时执行(原因在于JS模型的运转流程)
 * JS是单线程 所以定时器是在JS的主线程执行的
	- JS单线程模式: 用途有关. 否则需要处理很复杂的同步问题


**举个例子**:


	在执行异步代码的时候，如果定时器被正在执行的代码阻塞了，

	它将会进入队列的尾部去等待执行直到下一次可能执行的时间出现（可能超过设定的延时时间）。

	setTimeout 和setInterval 是有着本质区别的：setTimeout 这段代码会在每次回调函数执行之后至少需要延时“指定延迟毫秒值”再去执行（可能是更多，但是不会少）。

	但是setInterval会每隔“指定延迟毫秒值”就去尝试执行一次回调函数，不管上一个回调函数是不是还在执行。

----------

## JS引擎代码

#### JS引擎代码的分类
* 初始化代码
* 回调代码
#### JS引擎执行代码的基本流程
1. 先执行初始化代码: 包含一些特别的代码
  * 启动定时器
  * 绑定Dom事件监听
  * 发送ajax请求
2. 后执行回调代码: 处理回调逻辑
#### JS模型的重要组成部分
  * 事件管理模块(在分线程执行,由浏览器管理)
  * 回调队列 callback queue
	- 任务队列 task queue
	- 消息队列 message queue
	- 事件队列 event queue
  * 事件轮询 event loop (遍历读取回调队列中的回调函数执行)
#### JS模型的运转流程
  * 执行初始化代码, 将事件回调函数交给对应模块管理
  * 当事件发生时, 管理模块会将回调函数及其数据添加到回调列队中
  * **只有当初始化代码执行完后(时间不定), 才会遍历读取执行回调队列中的回调函数**
  
![](http://wx1.sinaimg.cn/mw690/824389a9ly1fe1t8gdpa0j20hk0faaep.jpg)