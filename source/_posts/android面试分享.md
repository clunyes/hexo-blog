---
title: android面试分享
date: 2017-02-08 17:31:32
tags:
---
    首先感谢各个公司给了我面试的机会
这段时间在看机会，去了几个面试，把面试到的题目和大家分享一下



### <font color='af8888'>葱X公司，个人觉得这次面试没啥收获</font>

1. jvm的工作原理

    A：网上搜了下，涉及面很广
    1. Java代码编译和执行的整个过程
    2. JVM内存管理及垃圾回收机制

    随便一个都很难理解的

2. 内存泄漏

    推荐一个很好的文章https://medium.com/freenet-engineering/memory-leaks-in-android-identify-treat-and-avoid-d0b1233acc8#.tny5y511b
    说明了几种容易泄漏的点，服务不关，内部类（网络请求的），匿名类（也是网络请求）

    depth是0的就是泄漏的
    异步线程对控件用弱引用 文章最后给出了一些建议，关于多线程其实rxjava是非常好的解决方案。


3. 用过哪些设计模式

    单例，适配，工厂，策略，代理，builder 然而面试官似乎不满意，不过我接触到的似乎这就这些了。。

### <font color='af8888'> 某AI公司，面试官非常有耐心，人非常好（好想跟他混。。。）</font>

1. oauth2.0授权的流程
    第三方应用去服务商获取app secret
    （A）用户打开客户端以后，客户端要求用户给予授权-----跳至服务商页面。

    （B）如果用户同意，那么登录，根据用户登录信息和app secret，服务商同意给予客户端授权accessToken。

    （C）客户端使用上一步获得的授权accessToken，向资源服务器申请获取资源。

    （D）资源服务器确认令牌无误，返回该accessToken的相关数据。

    2. 如何封装jni，或者说如何对应java调用的方法和so的方法

    对于传统的JNI编程来说，JNI方法跟Java类方法的名称之间有一定的对应关系，要遵循一定的命名规则，如下：
    - 前缀： Java_
    - 类的全限定名，用下划线进行分隔（_）
    - 方法名：getTestString
 
       
        package cn.mynewclouedeu.ui.activity;
        public class ActivityTest extends BaseActivity {
        ...
        pubic native boolean isRight(String str);
        ...
        }
        //so 对应方法
        boolean Java_cn_mycloudedu_ui_activity_ActivityTest_isRight(JNIEnv *env,jobject class,jstring str){//env是jni环境，class是java中的this
        ...
        }
    
    

3. IPC AIDL的简单实现
 IPC的各种方式 广播 AIDL bundle ContentProvider以及socket
    要理解aidl要首先理解binder
    Binder是什么：binder连接前台应用和service的桥梁
    具体aidl怎么实现，大家去百度吧。之后给大伙解释这个，容我先<font color='EE2C2C'>消化消化</font>，以后一定放一个我的理解。

4. tcp长连接

    

5. git命令，git rebase

    因为公司的原因一直在用svn，git都是我个人在玩。所以对于这个问题，还是一脸懵逼，看来还是要多学习。

6. surfaceview的特性

    SurfaceView和View最本质的区别在于，surfaceView是在一个新起的单独线程中可以重新绘制画面（双缓冲绘制）而View必须在UI的主线程中更新画面。
    
    SurfaceView控件适合内存耗费大、需要频繁刷新的场景，常用于显示游戏、动画、视频等。
    
    两个重要方法lockCanvas；unlockCanvasAndPost
    ps什么叫双缓冲：SurfaceView内部会有两块Buffer。调用lockCanvas之后，便可以在bufferA上绘图了。绘完之后，调用unlockCanvasAndPost将bufferA显示再屏幕上，
    随后调用lockCanvas，在bufferB上绘图，调用unlockCanvasAndPost将bufferB显示在屏幕上。repeat。

7. httpdns

    httpdns和dns有什么差别
    dns基于udp协议
    httpdns基于http协议
    httpdns：httpdns使用HTTP协议进行域名解析，代替现有基于udp的dns协议
    
    httpdns优势：防劫持，精准调度，使用场景也是app居多。
    下一个问题，怎么用httpdns--阿里云就提供了，但是要按流量收费。

8. art dalvik的区别

    art才用预加载机制，在apk安装时