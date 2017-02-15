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
    
    饿汉单例 懒汉单例

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

    采用心跳包来keep-alive
    keep-alive ：一个连接在２小时内没有任何动作，服务器就向客户机发送一个探测报文，对于客户机：
                    (1)正常运行，并从服务器可达，TCP响应正常，保活定时器复位(再次获得２小时)
                    (2)客户机崩溃,关闭或者正在重启，TCP无响应，75秒后超时，服务器稍后发送9个探测报文(共计１０个)，间隔都为75秒。TCP都没有响应，认为客户机已经关闭连接
                    (3)客户机崩溃并重启完成：服务器将收到一个探测响应，这个响应是一个复位，使得服务器终止这个连接。
                    (4)客户机正常运行，但是服务器不可达，探测无响应，10次探测后无响应，关闭

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

    art采用预加载机制，在apk安装时就编译完成，占用空间更多一些。dalvik是动态编译，每次启动都要编译。
    gc方面，dalvik是同步的，art是部分异步。
    
9. 事件分发
    
    三个关键方法：dispatchTouchEvent 是否消耗当前事件
    onInterceptTouchEvent
    onTouchEvent
    

10. layout绘制流程
    
### <font color='af8888'> 某宝公司的电话面试</font>

1. android生命周期介绍

    onCreate 
    onRestart activity从不可见状态切换成可见状态
    onStart activity已经可见了，但是还在后台
    onResume activity可见了
    onPause activity还是可见的，但是不能交互了
    onStop activity不可见
    onDestroy

2. activity的四种启动模式
    activity栈：可以理解为线性的路径：actA---->actB---->actC 那么abc是在同一个栈中
    standard 默认 重新创建
    singleTop 栈顶复用
    singleTask 栈内复用，会弹出目标activity之上的其余activity
    singleInstance 单独占用一个栈，其余与singleTask一致

3. 远程service的调用
    localService和remoteService,就是上文的ipc

4. activity和fragment的区别，fragment的优势
    fragment对比activity更灵活，activity+fragment可以理想的实现业务。
   
    生命周期如下图：
    ![](../../../../../img/lifecycle.png)

5. 最满意的模块和其中使用的设计模式
    恩。。
    
6. https
    
    http + tls/ssl 
    
    
## 单独说一下looper handler
- 每一个线程有一个Looper，looper内部有一个mq
- handler有它的特性，handler可以在任意线程发送消息，handler发送的消息都会发送到它关联的looper的mq中
（handler的创建会关联一个looper，如果不设置默认是当前线程的looper）
- looper会轮询自己的mq来处理消息

Looper.loop() looper开始工作

### <font color='af8888'> 某怡科技面试，android负责人技术不错</font>
1. 线程池具体实现有哪几种
    
    FixedThreadPool   线程池固定的线程池
    CachedThreadPool  线程数上限很大，一有任务就会执行，超时就会停止。
    全部闲置时，线程全部停止，此时线程池不占用内存
    ScheduledThreadPool  核心线程数固定，非核心线程数目不固定，闲置就会回收
    SingleThreadExecutor 单个线程的线程池，是同步的
    
2. onMeasure参数意义
    MeasureSpec 32位的值，高2位是SpecMode，低30位是SpecSize
    SpecMode为
    
    UNSPECIFIED 
    
    EXACTLY 
    
    AT_MOLT 
    
    这个之前看书就没有太懂，面试自然不会。。
    
    

3. 自定义绘图，旋转

4. 图片3级缓存

5. activity怎么启动的
    是不是想问app怎么启动的？

6. IntentService
    service是运行在主线程中的，所以service是不能进行耗时操作的
    可以执行耗时的任务的service，内部实现是HandlerThread和handler。HandlerThread是一个有Looper的Thread。
    正是由于HandlerThread，IntentService可以进行耗时操作，处理完所有逻辑之后，会自动关闭服务！（这么厉害的东西，之前都没用过）
    
7. 图片缓存回收的算法    

8. 并发场景，争夺资源

    synchronized