---
layout: post
title: 什么是OpenAPI
category: OpenAPI
tags: 设计
keywords: api设计，架构
---

**OpenAPI Specification**（之前称为Swagger Specification）是针对REST APIs的API定义格式。一份OpenAPI文档可以令你更完整的定义你的API，包括
* 可用路径（/users）以及该路径允许的操作（GET /users. POST /users）
* 每个操作的输入和输出参数
* 授权认证（Authentication）方法
* 联系方式，代码协议和使用条例等其他信息

OpenAPI文档可以使用YAML或者JSON编写。这两种格式很容易阅读并且利于程序读取。

完整的OpenAPI Specification可以在Github找到：[OpenAPI 3.0 Specification](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/3.0.0.md)