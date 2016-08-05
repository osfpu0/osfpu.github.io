---
layout: post
title: nginx学习记录
category: 技术
tags: 服务器
keywords: nginx
---
# Nginx 学习笔记
## 1. 概念
* 反向代理服务器
* 抗并发，nginx 处理请求是异步非阻塞的（epoll），多个请求对应一个进程
* 发音 engineX

## 2. 使用
	mac上brew install nginx 
	修改nginx.conf
	运行nginx即可
	重新载入命令 nginx -s reload
	
## 3. 其他
* [官方初学者指南](http://nginx.org/en/docs/beginners_guide.html)
* 一个主进程，多个处理进程，可配置
* 需要配置fastcgi以处理动态页面