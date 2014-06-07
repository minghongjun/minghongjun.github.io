---
layout: post
title: Android Adb及其它常见问题
categories: Android
tags: adb
---

## Adb connection Error远程主机强迫关闭了一个现有的连接

本文主要解决eclipse连接android手机时adb connection error的问题——reset adb.
 
环境为真机测试，偶尔会报如下错误
Java代码  收藏代码

    [2012-04-24 20:41:34 - DeviceMonitor]Adb connection Error:远程主机强迫关闭了一个现有的连接。  
    [2012-04-24 20:41:36 - DeviceMonitor]Connection attempts: 1  
    [2012-04-24 20:41:38 - DeviceMonitor]Connection attempts: 2  
    [2012-04-24 20:41:40 - DeviceMonitor]Connection attempts: 3  
    [2012-04-24 20:41:42 - DeviceMonitor]Connection attempts: 4  
    [2012-04-24 20:41:44 - DeviceMonitor]Connection attempts: 5  
    [2012-04-24 20:41:46 - DeviceMonitor]Connection attempts: 6  
    [2012-04-24 20:41:48 - DeviceMonitor]Connection attempts: 7  
    [2012-04-24 20:41:50 - DeviceMonitor]Connection attempts: 8  
    [2012-04-24 20:41:52 - DeviceMonitor]Connection attempts: 9  
    [2012-04-24 20:41:54 - DeviceMonitor]Connection attempts: 10  
    [2012-04-24 20:41:56 - DeviceMonitor]Connection attempts: 11  
    [2012-04-24 20:44:06 - ddms]ADB rejected shell command (ls -l /): closed  
    [2012-04-24 20:44:11 - ddms]ADB rejected shell command (ls -l /): closed  

之前都是重启eclipse解决，但偶尔还解决不了。对于真机需要拔掉数据线，关闭eclipse重启，重新连接手机解决。 

 

但由于eclipse实在过于笨重，关闭重启时间过长。找到另外一种解决方法：

eclipse中视图模式选择DDMS(还有常见的java和debug视图)， 显示Devices窗口，若无可通过选择window->show view->Devices显示，再选择下拉箭头中的reset adb。

<img src="/media/img/android-adb.jpg">

此时eclipse会再自动重试一次，输入Connection attempts：1即表示成功啦

## adb server is out of date. killing...

1:今天调试android的时候发现一个诡异的问题

    [html] view plaincopy
    C:\Users\xxxx>adb start-server   
    adb server is out of date.  killing...   
    ADB server didn't ACK   
    * failed to start daemon *   

adb 不管执行 shell devices 还是logcat 都会报错 

    [html] view plaincopy
    adb server is out of date.  killing...   

究其源就是adb server没启动 

到stackoverflow上查了一下 经过分析整理如下：

    [html] view plaincopy
    C:\Users\xxxx>adb nodaemon server   
    cannot bind 'tcp:5037'   

原来adb server 端口绑定失败 

继续查看到底是哪个端口给占用了

    [html] view plaincopy
    C:\Users\xxxxxx>netstat -ano | findstr "5037"   
      TCP    127.0.0.1:5037         0.0.0.0:0              LISTENING       4236   
      TCP    127.0.0.1:5037         127.0.0.1:49422        ESTABLISHED     4236   
      TCP    127.0.0.1:49422        127.0.0.1:5037         ESTABLISHED     3840   
      
打开任务管理器kill掉4236 这个进程。ok

## android 使用 POI 操作 Excel 07 报找不到Class

Android项目特定结构：它的项目下有个libs，把所有的Jar包放到这个文件夹下，不用引入就可以了，它会自动加载，如果手动建个文件夹，名字不是libs，把jar包放入里面引用的话，Android项目会认不到包

## Failure [INSTALL_FAILED_OLDER_SDK]

原先在1.6rc1上写的程序，拿到1.5的SDK重新编译后却不能用

abd install bin/xxx.apk

安装上去，提示错误是：

Failure [INSTALL_FAILED_OLDER_SDK]
 
找了一下，是这个原因：

打开源码目录下的AndroidManifest.xml文件，然后注释掉或者删除掉这行：

android:minSdkVersion="9"

## java.lang.NoClassDefFoundError: com.jayway.android.robotium.solo.Solo

在eclipse中进行如下设置

"Properties > Java Build Path > Libraries", delete "Android Dependencies" in my test project. 

"Properties > Java Build Path > Libraries", delete two robotium jar and click "Add JARs" to re-import robotium jar in my test project. 

"Properties > Java Build Path > Order and Export", check two checkbok of robotium jar (and othera you want exported.) 
Clean my test project. 

## robotium+ant 编译问题

robotium+ant的编译问题
用ant运行robotium的时候一直有一个问题，就是测试代码的编译问题，为什么直接运行Android SDK tools ant build.xml自带的test的时候，它是运行未编译过的代码，也就是一直运行的是旧的代码，必需先在eclipse里面手动右键运行Android test后，再ant 运行 test 运行的才是最新的代码，试过ant 里面自带的compile,debug，然后再运行test,从bin目录中看到，运行compile或者debug后，测试代码确实也编译成功，但是运行test的时候运行的还是旧的代码。这个问题一直困扰了我好久，网上查了好入也没有找到问题的办法，今天早上心血来潮看了下eclipse手动运行测试的时候logcat，发现原来Android在运行测试代码的时候，在打apk包的时候，会把测试代码一起更新到模拟器上，所以我们在运行test之前还要先把新得到的apk安装到虚拟机器上，build.xml里面有自带的方法：installd，运行debug,然后installd，然后test，这个时候运行的就是最新的代码。
虽然是个很简单很傻X的问题，却花了我这么久的时间来解决。

ant debug: Builds the application and signs it with a debug key. 
ant installd:install the tests and apk against the instrumented code
ant run-tests: Runs tests from the package defined in test.package property








