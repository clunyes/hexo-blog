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

    随便一个都很难理解的。

2. 内存泄漏

    推荐一个很好的文章https://medium.com/freenet-engineering/memory-leaks-in-android-identify-treat-and-avoid-d0b1233acc8#.tny5y511b
    说明了几种容易泄漏的点，
    
    服务不关，内部类（网络请求的），匿名类（也是网络请求） 网络请求，退出务必关闭回收或者取消。
    异步线程对控件等用弱引用的方式引用

    depth是0的就是泄漏的
     
    文章最后给出了一些建议，关于多线程其实rxjava是非常好的解决方案。


3. 用过哪些设计模式

    单例，适配，工厂，策略，代理，builder 然而面试官似乎不满意，不过我接触到的似乎这就这些了。。
    
    饿汉单例 懒汉单例 饿汉单例比懒汉单例问题少，涉及到多线程，加上synchronize。

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
 IPC的各种方式 广播 AIDL Messenger bundle ContentProvider以及socket
    要理解aidl要首先理解binder
    Binder是什么：binder连接前台应用和service的桥梁
    [具体aidl怎么实现](http://clunyes.github.io/2017/2/20/AIDL的梳理.md/).

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
    
    1. 如果没有DecorView就创建一个（DecorView包含标题栏和内容栏（经大神验证，标题栏不是状态栏statusBar））
    2. 将你的xml，添加到decorView的内容栏contentPatent（R.id.content）
    3. activity回调onContentChanged
    
    结构就是DecorView--> ViewGroup（DecorView的内容栏）--> 你的View。
    
    说道这里，必须说一下activity window和view的故事
    
        我们知道view是视图，但是android中view不能单独存在，他必须依附于window之上，window就是负责展示view的。
        activity负责控制window的行为，同时window的结果都会回调给activity。
        那么view和window是怎么bind起来的呢，通过ViewRootImpl，它就是window和view交互的桥梁。
    
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
    
    activity栈：可以理解为线性的路径：actA---->actB---->actC 那么abc是在同一个栈中
    
    正常standard启动模式的activity默认会进入启动它的Activity所属的任务栈中
    
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

[深入探讨下线程](http://clunyes.github.io/2017/2/27/javaThread状态.md/).

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
    
    startActivity最终会调用startActivityForResult
    
    1. 获取要启动的activity的信息，
    2. 由Instrumentation创建activity对象，
    3. 创建application对象，
    4. 创建ContextImpl对象并调用attach完成重要数据初始化，
    5. activity onCreate； 具体过程远比这个复杂的多，这个仅仅是最后一步

6. IntentService
    service是运行在主线程中的，所以service是不能进行耗时操作的
    可以执行耗时的任务的service，内部实现是HandlerThread和handler。HandlerThread是一个有Looper的Thread。
    正是由于HandlerThread，IntentService可以进行耗时操作，处理完所有逻辑之后，会自动关闭服务！（这么厉害的东西，之前都没用过）
    
7. 图片缓存回收的算法    

8. 并发场景，争夺资源

    synchronized -- 
    
### <font color='af8888'> 扩展问题</font>

1. 数据结构有哪些

        1. 逻辑结构分为:集合结构；线性结构；树形结构；图形结构
        2. 物理（计算机存储）结构：顺序存储结构，数组；链式存储结构，链表
        
 数组 
 
 链表
 
 栈

 队列
 
 树
 
 图

2. java atomic原子  volatile copyOnWrite思路，各种集合类的实现

3. [android 热修复原理](http://clunyes.github.io/2017/2/22/android热修复原理.md/)
    
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

