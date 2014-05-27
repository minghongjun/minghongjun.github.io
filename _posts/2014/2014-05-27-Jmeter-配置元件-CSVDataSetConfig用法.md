---
layout: post
title: jmeter 配置元件 CSV Data Set Config用法
categories: jmeter
tags: 
    - jmeter
    - csv
---

CSV Data Set Config的用法，如下图：

<img src="/media/img/jmeter-csv.jpg">

Filename：文件的具体地址以及名称如：E:\forTest\data\a.txt ；
File encoding :给出页面的编码方式，这里以百度为例，它的源代码里

    <meta http-equiv="content-type" content="text/html;charset=gb2312"> 

所以这里File encoding:gb2312

`Variable Names(comma-delimited)`:给出变量名如：a,b

这里的变量名是给后面引用用的，如要用到这个文件的值，可以利用变量名来引用：${a},${b},如a.txt文件中有这样的数据:jmeter,ant,那${a}就可以引用到jmeter,${b}就可以引用到ant

`Delimiter(use '\t' for Tab)`:这个是用来隔开变量的分隔符，如上面的a,b,那分隔符就是“,”

综上：CSV Data Set Config实现的功能跟之前用的：`${__CSVRead(${test.data.folder}/上架出售.txt,0)}`这个函数实现的功能大体上是一样的。不过`${__CSVRead(${test.data.folder}/上架出售.txt,0)}`这个函数还要在jmeter bin下的user.properties文件里面配置test.data.folder，然后在jmter 里面配置全局变量：test.data.folder的值为：`${__property(test.data.folder)}`