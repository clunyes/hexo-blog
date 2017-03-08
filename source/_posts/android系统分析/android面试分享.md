---
title: android面试分享
date: 2017-02-08 17:31:32
tags:
---
    首先感谢各个公司给了我面试的机会
这段时间在看机会，去了几个面试，把面试到的题目和大家分享一下

### <font color='af8888'> 某AI公司，面试官非常有耐心，人非常好（好想跟他混。。。）</font>

1. oauth2.0授权的流程
    第三方应用去服务商获取app secret
    （A）用户打开客户端以后，客户端要求用户给予授权-----跳至服务商页面。

    （B）如果用户同意，那么登录，根据用户登录信息和app secret，服务商同意给予客户端授权accessToken。

    （C）客户端使用上一步获得的授权accessToken，向资源服务器申请获取资源。

    （D）资源服务器确认令牌无误，返回该accessToken的相关数据。
    
2. [jni调用](http://clunyes.github.io/2017/03/03/java深入学习/jni浅析/).

3. IPC AIDL的简单实现
 IPC的各种方式 广播 AIDL Messenger bundle ContentProvider以及socket
    要理解aidl要首先理解binder
    Binder是什么：binder连接前台应用和service的桥梁
    [具体aidl怎么实现](http://clunyes.github.io/2017/02/20/android系统分析/AIDL的梳理/).

4. tcp长连接

    采用心跳包来keep-alive
    keep-alive ：一个连接在２小时内没有任何动作，服务器就向客户机发送一个探测报文，对于客户机：
                    (1)正常运行，并从服务器可达，TCP响应正常，保活定时器复位(再次获得２小时)
                    (2)客户机崩溃,关闭或者正在重启，TCP无响应，75秒后超时，服务器稍后发送9个探测报文(共计１０个)，间隔都为75秒。TCP都没有响应，认为客户机已经关闭连接
                    (3)客户机崩溃并重启完成：服务器将收到一个探测响应，这个响应是一个复位，使得服务器终止这个连接。
                    (4)客户机正常运行，但是服务器不可达，探测无响应，10次探测后无响应，关闭

5. git命令，git rebase

    因为公司的原因一直在用svn，git都是我个人在玩。所以对于这个问题，还是一脸懵逼，看来还是要多学习。

6. surfaceView的特性

    SurfaceView和View最本质的区别在于，surfaceView是在一个新起的单独线程中，可以重新绘制画面（双缓冲绘制）而View必须在UI的主线程中更新画面。
    
    SurfaceView控件适合内存耗费大、需要频繁刷新的场景，常用于显示游戏、动画、视频等。
    
    两个重要方法lockCanvas；unlockCanvasAndPost
    ps什么叫双缓冲：SurfaceView内部会有两块Buffer。调用lockCanvas之后，便可以在bufferA上绘图了。绘完之后，调用unlockCanvasAndPost将bufferA显示再屏幕上，
    随后调用lockCanvas，在bufferB上绘图，调用unlockCanvasAndPost将bufferB显示在屏幕上。repeat。

7. httpdns

    httpdns和dns有什么差别
    httpdns基于http协议
    httpdns：httpdns使用HTTP协议进行域名解析，代替现有基于udp的dns协议
    
    httpdns优势：防劫持，精准调度，使用场景也是app居多。
    下一个问题，怎么用httpdns--阿里云就提供了，但是要按流量收费。

8. art dalvik的区别

    art采用预加载机制，在apk安装时就编译完成，占用空间更多一些。dalvik是动态编译，每次启动都要编译。
    gc方面，dalvik是同步的，art是部分异步。
    
9. 事件分发
    
    三个关键方法：dispatchTouchEvent 是否消耗当前事件
    onInterceptTouchEvent 是否拦截当前事件
    onTouchEvent 是否消耗当前事件
    
    那么这三个方法有啥关系呢
    
        public  boolean dispatchTouchEvent(MotionEvent ev){
            boolean consume = false;
            if(onInterceptTouchEvent(ev)){
                consume = onTouchEvent(ev);
            } else{
                consume = child.dispatchTouchEvent(ev);
            }
            return consume;
        }
        
    有些要点需要记住，一个事件序列只能被一个view消耗，
    
    如果一个事件没有view去处理，那么事件会交给activity处理
    
    ViewGroup的onInterceptTouchEvent默认不调用
    
    view没有onInterceptTouchEvent的方法
    
    事件传递是由activity--window--view。

10. layout绘制流程

    activity attach方法，创建window建立关联。setContentView
    
    1. 如果没有DecorView就创建一个（DecorView包含标题栏和内容栏（经大神验证，标题栏不是状态栏statusBar））
    2. 将你的xml，添加到decorView的内容栏contentPatent（R.id.content）
    3. activity回调onContentChanged
    4. 最后在onResume中调用makeVisible
    
    结构就是DecorView--> ViewGroup（DecorView的内容栏）--> 你的View。
    
    说道这里，必须说一下activity window和view的故事
    
        我们知道view是视图，但是android中view不能单独存在，他必须依附于window之上，window就是负责展示view的。
        activity负责控制window的行为，同时window的结果都会回调给activity。
        那么view和window是怎么bind起来的呢，通过ViewRootImpl，它就是window和view交互的桥梁。
        
11. jvm

    详见 http://www.jianshu.com/p/54eb60cfa7bd
    
12. 内存泄漏
    
        推荐一个很好的文章https://medium.com/freenet-engineering/memory-leaks-in-android-identify-treat-and-avoid-d0b1233acc8#.tny5y511b
        说明了几种容易泄漏的点，归总也就是一点activity或者fragment中不要有超过本身生命周期的引用，
        否则activity（fragment）无法被回收。
        
        1服务不关，2内部类（网络请求的），3匿名类（也是网络请求） 网络请求，退出务必关闭回收或者取消。
        异步线程对控件等用弱引用的方式引用
    
        depth是0的就是泄漏的
         
        文章最后给出了一些建议，关于多线程其实rxjava是非常好的解决方案。
    
### <font color='af8888'> 某宝公司的电话面试</font>

1. android生命周期介绍

    onCreate 
    onRestart activity从不可见状态切换成可见状态
    onStart activity已经可见了，但是还在后台
    onResume activity可见了
    onPause activity还是可见的，但是不能交互了
    onStop activity不可见
    onDestroy
    
    onPause和onStop一般一起触发

2. activity的四种启动模式

    先介绍两个概念task和taskAffinity，
    
        task：翻译过来就是“任务”，是一组相互有关联的 activity 集合，可以理解为 Activity 是在 task 里面活动的。 
        task 存在于一个称为 back stack 的数据结构中，也就是说， task 是以栈的形式去管理 activity 的，
        所以也叫可以称为“任务栈”。
        taskAffinity：官方文档解释是："The task that the activity has an affinity for."，
        可以翻译为 activity 相关或者亲和的任务，这个参数标识了一个 Activity 所需要的任务栈的名字。
        默认情况下，所有Activity所需的任务栈的名字为应用的包名。 taskAffinity 属性主要和 singleTask 
        启动模式或者 allowTaskReparenting 属性配对使用。
    
    正常standard启动模式的activity默认会进入应用报名所属的任务栈中
    
    standard 默认 重新创建
    
    singleTop 栈顶复用，如果栈顶就是这个Activity，那么onNewIntent会被调用
    
    singleTask 栈内复用，在第一次启动这个 Activity 时，系统便会创建一个新的任务，taskAffinity 属性是和 singleTask 模式搭配使用的。
    。如果再次调用会弹出目标activity之上的其余activity，回调onNewIntent。还有个特殊参数allowTaskReparenting，如果是true，会转移任务栈。
    
    singleInstance 单独占用一个栈，其余与singleTask一致

3. 远程service的调用
    localService和remoteService,就是上文的ipc

4. activity和fragment的区别，fragment的优势
    fragment对比activity更灵活，fragment轻量级，更加易于管理，易于适配手机和平板。
   
    生命周期如下图：
    ![](../../../../../images/lifecycle.png)

5. 最满意的模块和其中使用的设计模式
    恩。。
    
6. https
    
    http + tls/ssl 
    
    
## 单独说一下looper handler
- 每一个线程有一个Looper，looper内部有一个mq
- handler有它的特性，handler可以在任意线程发送消息，handler发送的消息都会发送到它关联的looper的mq中
（handler的创建会关联一个looper，如果不设置默认是当前线程的looper）
- looper会轮询自己的mq来处理消息，交给handler的handleMessage去处理。

Looper.loop() looper开始工作

[深入探讨下线程](http://clunyes.github.io/2017/02/27/java深入学习/javaThread状态/).

[java线程池](http://clunyes.github.io/2017/02/28/java深入学习/java线程池/).

### <font color='af8888'> 某怡科技面试，android负责人技术还行，</font>感觉是对着android开发艺术探索来面的，不过这本书确实经典

1. 线程池具体实现有哪几种
    
    FixedThreadPool   线程池固定的线程池
    CachedThreadPool  线程数上限很大，一有任务就会执行，终止并从缓存中移除那些已有 60 秒钟未被使用的线程。
    全部闲置时，线程全部停止，此时线程池不占用内存
    ScheduledThreadPool  核心线程数固定，非核心线程数目不固定，闲置就会回收。适用于执行定时任务，固定周期的任务。
    SingleThreadExecutor 单个线程的线程池，是同步的
    
2. onMeasure参数意义
    MeasureSpec 32位的值，高2位是SpecMode，低30位是SpecSize
    
    SpecMode为
    
    UNSPECIFIED 父容器不对view有任何限制
    
    EXACTLY 父容器已经检测出view所需要的具体大小，最终大小就是specSize
    
    AT_MOLT 父容器指定了一个可用的specSize，view的大小不能超过这个specSize
    
    LayoutParam和父容器一起决定MeasureSpec:
    
    layoutParam.match_parent 大小就是窗口大小
    
    layoutParam.wrap_content 大小不定，不能超过窗口大小
    
    100dp固定大小：为layoutParam指定的大小 
    
    以上为基础知识
    
    子元素measureSpec创建和父容器measureSpec和子元素本身的LayoutParam，还有子元素的margin padding有关系
    
    在写自定义view时，自身大小为具体size,view的specMode都是EXACTLY为childSize，如果是match_content 那么不论是EXACTLY还是AT_MOST都是parentSize，
    如果要自身大小为wrap_content，则需要重写onMeasure方法，因为在wrap_content时，view的specMode都是AT_MOST，即会填满父控件，这不是我们想要得效果
    如何重写，在onMeasure中判断AT_MOST，将宽高用默认宽高代替。
   
3. 自定义绘图，旋转

    采用matrix.setRotate的方式，matrix bitmap的关系？

4. 图片3级缓存
    
    网络缓存, 不优先加载, 速度慢,浪费流量
    本地缓存, 次优先加载, 速度快
    内存缓存, 优先加载, 速度最快

5. activity的启动过程
    
    [详细说明](http://clunyes.github.io/2017/02/24/android系统分析/activity启动和service启动/)

6. IntentService
    service是运行在主线程中的，所以service是不能进行耗时操作的
    可以执行耗时的任务的service，内部实现是HandlerThread和handler。HandlerThread是一个有Looper的Thread。
    正是由于HandlerThread，IntentService可以进行耗时操作，处理完所有逻辑之后，会自动关闭服务！（这么厉害的东西，之前都没用过）
    
7. 图片缓存回收的算法    

        1. 新数据插入到链表头部；
        
        2. 每当缓存命中（即缓存数据被访问），则将数据移到链表头部；
        
        3. 当链表满的时候，将链表尾部的数据丢弃。
    
    LRU，最近最少使用算法
    LruCache是由LinkedHashMap来实现的，同步实现（因为是不同线程调用的）。

    图片缓存的是drawable。

8. 并发场景，争夺资源

    [synchronized](http://clunyes.github.io/2017/03/01/java深入学习/java线程安全/)
    
### <font color='af8888'> 扩展问题</font>

1. 数据结构有哪些

        1. 逻辑结构分为:集合结构；线性结构；树形结构；图形结构
        2. 物理（计算机存储）结构：顺序存储结构，数组；链式存储结构，链表
        
 数组 String[] 包括列表
 
 链表 LinkedList
 
 栈 activity task 就是一个栈，先进后出

 队列  基本上，一个队列就是一个先入先出（FIFO）的数据结构
 
 树
 
 图

2. java  copyOnWrite思路，
    [各种集合类的实现](http://clunyes.github.io/2017/02/28/java深入学习/java集合理解/)

3. [android 热修复原理](http://clunyes.github.io/2017/02/22/android热修复原理/)
    
4. android保活
    1. 不同app利用广播相互唤醒
    
    2. 开启前台服务，不显示notification--这种方式还是会被杀死的
    
    3. 如果够牛逼，进入系统的白名单
    
    4. 一个像素的activity
    
    5. JobScheduler（有待验证怎么使用的）
    
    进程回收机制：系统内存不足的情况下android会开始杀进程，所使用的机制就是Low Memory Killer。
    
    oom_adj值越低越不会被杀死
    
    app退到后台时，其所有的进程优先级都会降低。但是UI进程是降低最为明显的，因为它占用的内存资源最多，
    系统内存不足的时候肯定优先杀这些占用内存高的进程来腾出资源。所以，为了尽量避免后台UI进程被杀，
    需要尽可能的释放一些不用的资源，尤其是图片、音视频之类的。
    
    目前来讲，app进入到后台后，如果资源紧张肯定会被kill。
    
5. 生产者消费者模式

6. android优化
    
    1. 内存泄漏方面
    2. ui视图绘制慢导致卡顿
    3. 严苛模式
    4. 启动优化--不必要在application中初始化的代码，放到ui初始化完成后。
    
7. socket的基本流程
    ![](../../../../../images/socket主流程.png)
    
    
### <font color='af8888'> 唯X科技面试，</font>有一句说一句，面试官是架构师技术非常不错，人很谦和，

面试官表示我还是个中级。哎，慢慢面吧。

1. android ndk 开发

2. 对于android framework的理解

3. 源码要自己敲一遍，理解更加深刻

4. app启动时间优化 adb shell am start -W cn.mynewclouedeu/cn.mynewclouedeu.ui.activity.ActivityGuide 
查出启动时间，application初始化使用线程操作。

5. java与模式，理解低耦合高内聚

6. 动态注册广播比静态注册广播先收到广播

7. 图片压缩，高清晰度，仿微信

8. aop 面向切面编程

9. 增量更新

10. 注解的理解

这道问题非常考验功力，慢慢修炼。

### <font color='af8888'> X联动力面试，</font>面试流程比较奇葩的，提了个需求问行不行
1. 需要要和硬件打交道（jni hal你都得会）

2. 自己公司有自己的视频流协议（视频流的协议，通信协议你得熟悉）

3. 要做android app开发（app相关你也得过关）

要求还是挺高的，如果去做的话对于我这样的工程师估计够呛。


顺利的面试我就不发了，目标还是定在大厂，不是大厂也得是大牛领导，实在是不想再去坑爹公司。   
    
又发现了一个大牛的面经： http://www.jianshu.com/p/dea7c3555b3c    ，不得不说基础很重要。