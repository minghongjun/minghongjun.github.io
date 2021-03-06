---
layout: post
title: loadrunner 事务和集合点 参数 分析测试结果
categories: loadrunner
tags: 
    - parameter
    - Transaction
---

## 参数

LR参数：多个参数用一个表里面的不同列
如username,password是两个关联的值

其实是一个dat文件中2个列分别代表username和password这2个参数，调用时分别用｛username｝｛password｝调用就行了，参数设置见附件的截图

在Vuser下打开Parameter List：

<img src="/media/img/loadrunner-para-1.jpg">

<img src="/media/img/loadrunner-para-2.jpg">

## 参数

首先，为了衡量服务器的性能，需要定义事务（Transaction），例如在脚本中有一个数据查询操作，为了衡量服务器执行查询操作的性能，可以把这个操作定义为一个事务。这样在运行测试脚本时，LoadRunner运行到该事务的开始点时，就会开始计时，直到运行到该事务的结束点，计时结束。这个事务的运行时间在测试结果中会有反映。 LoadRunner允许在脚本中插入不限数量的事务。

其次，使用集合点是为了衡量在加重负载的情况下服务器的性能情况。在测试计划中，可能会要求系统能够承受多人同时提交数据，LoadRunner通过在提交数据操作前面加入集合点的方法，检查同时有多少用户运行到集合点，人数不足时，LoadRunner会命令已到集合点的用户等待，当在集合点等待的用户达到要求容纳的人数（如1000人）时，LoadRunner向系统提交数据。

再次，在录制过程中最了加入注释，因为在录制完脚本后看到的都是脚本代码，操作复杂的业务无法找到相应的位置进行关联或者参数化的动作，这时，注释就显得尤为重要。

最后，LoadRunner提供了很我函数，有些函数是在录制时根据不同的协议自带的函数。其中有些函数是供手工添加的，这就要根据实际情况进行添加了。例如脚本关联，有些变量无法实现系统自动关联，只能添加函数进行手动关联。

## 分析测试结果

用以下几种方法分析lr获取的性能测试数据：

	1、查看Vuser Log文件，这些文件包括了场景运行过程中每个用户的跟踪数据，Vuser  Log文件一般放在脚本目录中；

	2、在控制台的输出窗口查看场景的执行过程信息；

	3、使用Analysis模块分析执行结果图表；

	4、使用直接生成的图表查看原始数据——Graph Data或者Raw Data；

	5、让LR自动生成HTML或Word格式的测试报告，通过报告进行分析。

