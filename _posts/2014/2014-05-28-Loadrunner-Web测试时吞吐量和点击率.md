---
layout: post
title: loadrunner Web测试时吞吐量和点击率
categories: loadrunner
tags: 
    - Throughtput
    - hits
---

首先先了解一下浏览器的工作原理，简单的说，当用户访问某个Html文件时，浏览器首先获得该Html文件，然后进行语法分析。如果这个Html文件包含图片、视频等信息，浏览器会再次访问后台的Web服务器，一次获取这些图像、视频文件，然后把Html和图像、视频文件组装起来，显示在屏幕上。

在LoadRunner的分析报告中，有Total Throughtput(bytes)和Total Hits两个重要的指标。我们简称为吞吐量和点击量。那么这两个指标的是怎么计算出来的呢？

例如页面<http://xxxx.com/default.aspx?keyword=新魔界>。如下图，用HttpWatch扫一下，

<img src="/media/img/loadrunner-hits-1.jpg">

每访问一次该页面，发生了一次“请求----接收”，发送了492 bites,接收了1593 bites,那么在一次访问中，吞吐量是1593 bites，还是492 bites+1593 bites呢？访问一次，点击量就是一次吗？

下面的实例就是用LR录制上面的访问行为，进行压力测试，看看分析报告中给出的数据到底是怎么计算的。

用LR录制脚本，选择HTTP协议。LR录制Web脚本有HTML模式和URL模式。用HTML模式录制是基于用户行为模拟，为每个用户请求生成单独的函数，而基于URL模式则与基于用户行为模拟不同的是不考虑任何用户操作，只考虑客户端发送的请求，注重于实际上系统做了什么。分别录制这两种模式的脚本：

1.例如页面http://xxx.com/default.aspx?keyword=新魔界

HTML模式录制的脚本如下图：

<img src="/media/img/loadrunner-hits-2.jpg">

URL模式录制的脚本如下图：

<img src="/media/img/loadrunner-hits-3.jpg">



