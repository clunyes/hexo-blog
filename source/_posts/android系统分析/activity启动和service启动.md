---
title: activity启动和service启动
date: 2017-02-24 13:49:03
tags:
---
对于android最重要的两个核心组件activity service研究也是比较多的

浅显的问题1： startService和bindService有什么区别：
    
    startService()方式启动，Service是通过接受Intent并且会经历onCreate()和onStartCommand()。当用户在发出意图使之销毁时会经历onDestroy()。
    而bindService()方式启动，与Activity绑定的时候，会经历onCreate()和onBind()，而当Activity被销毁的时候，Service会先调用onUnbind()然后是onDestroy()。

    使用startService()方法启用服务，调用者与服务之间没有关连，即使调用者退出了，服务仍然运行。
    使用bindService()方法启用服务，调用者与服务绑定在了一起，调用者一旦退出，服务也就终止。

深入的问题1:
Binder优势：
Android需要建立一套新的IPC机制来满足系统对通信方式，传输性能和安全性的要求，这就是Binder。
http://blog.csdn.net/universus/article/details/6211589 这篇文章可以反复阅读。

    Binder通信的四个角色：

        Client进程：使用服务的进程。
        Server进程：提供服务的进程。
        ServiceManager进程：ServiceManager的作用是将字符形式的Binder名字转化成Client中对该Binder的引用，使得Client能够通过Binder名字获得对Server中Binder实体的引用。
        Binder驱动：驱动负责进程之间Binder通信的建立，Binder在进程之间的传递，Binder引用计数管理，数据包在进程之间的传递和交互等一系列底层支持。



那么他们的启动流程到底是怎么样的？

看到一篇非常好的文章
activity启动流程文章： http://www.jianshu.com/p/e0a6717bc75e 
，我就不献丑了

基本流程-两个ipc，应用进程和system server进程，应用进程里涉及到的线程包括ui线程和ApplicationThread

    Activty---ActivityManagerProxy : ActvitiyManagerNative--ActivityManagerService
    
    ActivityManagerService--ApplicationThreadProxy : ApplicationThreadNative--ApplicationThread-(H handler)-ActivityThread

service和activity流程基本差不多

我这里介绍下一些关键的类
   
    ActivityManagerService （很多都是简称ams，类似的重要服务还有很多PackageManagerService，WindowManagerService）
    相当于BookManagerService，是服务端。
    
    服务端提供哪些接口？IBookManager中说明提供了哪些接口。
    
    ActivityManagerNative
    怎么和服务端通信呢，要一个AIDL的接口，这个相当于IBookManager.Stub。
    
    ActivityManagerProxy
    具体方法的实现在呢，Proxy就是具体实现了
    
    Instrumentation
    工具类
    
[具体aidl怎么实现](http://clunyes.github.io/2017/2/20/android系统分析/AIDL的梳理.md/)请看这里.    
    
关于AMS，AMS并不是字面意思Activity管理服务，而是service也在其中管理。
    
启动Activity至少需要两个前提，第一是，应用进程存在，第二AMS已经初始化完毕。