---
layout: article
title: 关于浏览器内核
keywords: 浏览器内核,常见的浏览器内核,浏览器内核的组成
description: 浏览器内核主要分成两部分：渲染引擎(layout engineer或 Rendering Engine) 和 JS 引擎
date: 2019-05-20 00:00:00 Z
categories: other
---


## 浏览器内核的组成部分

> 主要分成两部分：渲染引擎(layout engineer或 Rendering Engine) 和 JS 引擎。

- 渲染引擎：负责取得网页的内容（HTML、 XML 、图像等等）、整理讯息（例如加入 CSS 等），以及计算网页的显示方式，然后会输出至显示器或打印机。浏览器的内核的不同对于网页的语法解释会有不同，所以渲染的效果也不相同。所有网页浏览器、电子邮件客户端以及其它需要编辑、显示网络内容的应用程序都需要内核。

- JS引擎：解析和执行 JavaScript 来实现网页的动态效果。

---

## 常见的浏览器内核

### WebKit内核

> 代表浏览器有Safari、Chromewebkit。是一个开源项目，包含了来自KDE项目和苹果公司的一些组件，主要用于Mac OS系统，特点在于源码结构清晰、渲染速度极快。缺点是对网页代码的兼容性不高，导致一些编写不标准的网页无法正常显示。主要作品有Safari以及Chrome。

### Gecko内核

> 以Mozilla浏览器为代表，FirefoxGecko是一套开放源代码的、以C++编写的网页排版引擎。Gecko是最流行的排版引擎之一。使用它的比较著名的浏览器是Firefox、Netscape6至9.

### Presto内核

> 是由Opera Sofeware开发的浏览器排版引擎（已废弃）：该款引擎的特点就是渲染速度的优化达到了极致，然而代价却是牺牲了网页的兼容性。

### Trident内核

> Trident又被称为MSHTML，是微软开发的一种排版引擎。使用Trident渲染引擎的浏览器主要有：IE、世界之窗浏览器、傲游、Avant、Sleipnir、GreenBrowser、NetCaptor和KKman等。