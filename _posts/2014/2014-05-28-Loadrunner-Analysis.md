---
layout: post
title: loadrunner Analysis
categories: loadrunner
tags: Analysis
---

## Analysis简介

lr 的Analysis模块是分析系统和性能指标的一个主要工具，它能够直接打开场景的执行结果文件，将场景数据信息生成相关的图表显示出来。 Analysis集成了数据统计分析功能，允许测试人员对图表进行比较和合并等多种操作，分析后的图表能够自动生成测试员需要的测试报告文档。

在运行方案时，数据将存储在结果文件中，扩展名为.lrr。Analysis是处理收集的结果信息并生成图和报告的实用程序。Analysis将活动图的显示信息和布局设置存储在扩展名为.lra的文件中。

## Analysis报告中最重要和最有趣的部分是事务概要：check_itinerary

可以在Runnering Vuser图筛选：右键单击Runnering Vuser图，选择：“设置筛选器/分组方式”，或者单击工具栏上的“设置筛选器/分组方式”图标，选定时间，筛选。

可以把两个图放在一起，以查看一个图的数据对另一个图的数据所产生的影响，这称为图的关联。例：您可以将上面筛选后的图和平均事务响应时间图相关联，以查看大量的Vuser对事务的平均响应时间产生的影响。

右键单击正在运行的Vuser图并选择“合并图”。

在“选择要合并的图”列表中，选择“平均事务响应时间”。

在“选择合并类型”区域中，选择“关联”，然后单击“确定”。

现在，正在运行的Vuser图和平均事务响应时间图在图查看区域中表示为一个图，即：正在运行的Vuser—平均事务响应时间图

## Analysis 事物统计的Std.Deviation

Std是单词standard的缩写，Std.Deviation即标准方差，方差是描述一组数据偏离其平均值的情况。

从数字意义上看，方差值越大，这组数据就越离散，波动性也越强；方差越小，这组数据就越聚合，波动性也就越小。

提示：显然，方差这个数字能说明一些问题，比如我们如果试图比较一场景的先后两次运行结果，在Average相差比较大的情况下，我们可以通过Std.Deviation值来判断这个变化是由一些随机因素引起的，还是确定调优或修改代码生效了。如果方差小，说明这些数据凝聚度高，随机因素干扰小；方差大，则相反。因此可以说在事物统计信息里，相关方差值越小，这组数据越好，越有说服力。

## 详解FirstBufferTime

详解 第 一次缓冲时间 测试结果分析过程中，经常遇到第一次缓冲时间 FirstBufferTime,并且发现大部分系统的响应时间也都浪费在了这里，再给研发解释这个问题时候，又不能拿FirstBufferTime 直接给研发说，抽时间整理了下，希望对大家有用

以下资料来自 LR 帮助手册：

定义： 第一次缓冲时间细分图显示成功从 Web 服务器返回的第一次缓冲之前的这一段时间内，每个网页组件的相关服务器/网络时间（以秒为单位）

网络时间：从发送第一个 http 请求那一刻直到收到确认为止，所经过的平均时间

服务器时间：从收到初始 http 请求确认（通常为 get）直到成功收到来自 Web 服务器的第一次缓冲为止，所经过的平均时间。

注意：要从客户端测定服务器时间，因此发送初始 HTTP请求到发送第一次缓冲这一段时间内网络性能发生变化，则网络时间可能会影响此度量，因此所显示的服务器时间是一个估计值，可能不太精确

如果细心看了后面的注意，很多人就明白了，所有关于 FirstBufferTime时间的度量都是来自客户端的。至于帮助手册里面的不太精确，还有这个网络时间和服务器时间如何计算，我分析了下，个人观点如下：

首先要了解 http 传输，这个大叔以前发过相关资料，

这里推荐个网址 <http://www.cnpaf.net/Class/HTTP/200408/83.html>

里面有个形象的举例，就是电话订货，到送货上门， 这里拿图讲例子;

<img src="/media/img/loadrunner-firstbuffer.jpg">

上图类似于 http 链接 请求响应模式，其中 很明显

网络时间=N1+N2+N3

响应时间=R1+R2

而根据 LR 结果分析定义发现

 网络时间=N1+R1+N2

响应时间=R2+N3
 
怎么解释呢 ？N1 过程为建立链接过程，就是客户端给服务器端发送请求说，我 要取什么东西，R1 为服务器响应时间，响应结果只是收到，然后经过 N2 传输给 客户端，OK 链接建立，但是服务器响应还没结束，要准备客户端所索取的数据，所以继续响应，（此时对 http 来说，客户端处于等待接收状态），然后把结果 传输给客户端，这就是 N3 时间

LR 在计算结果时候 拿 R1 和 N3 进行了抵消，所以给出来的只是近似值 


