---
title: OkHttp学习
date: 2016-11-28 11:56:59
tags:
---
#### 首先我拿OkHttpClient作为切入点
* 内部类Builder，典型的builder模式
主要参数包括 

1. dispatcher 执行请求的线程池

2. proxy （代理）

3. protocols （协议）

interceptors 拦截器，在请求发送之前加入

connectTimeout 链接超时时间，还有读超时，写超时时间

followRedirects 自动跳转

cookie 

client内部的参数都是依靠builder传入的。

还有各种各样的验证，功能很多，我都没见过。那么client是干吗用的。response = client.execute(request)。这样写大家应该有点明白了，引入了下面两个主角request response。

* request 同样使用builder模式（builder模式，用于对象特别复杂的时候，利于用户灵活使用）
主要参数包括

1. url 
2. method
3. headers 传入键值对namesAndValues
4. body requestBody 用于post方法
5. tag （用于request取消，之后验证）

* response 毫无意外，依旧builder

1. request 请求
2. 一些http状态信息
3. handshake 如果是3次握手的tcp，还有这个信息
4. headers 返回头
5. 网络请求响应和缓存的响应

关键的问题来了response = client.execute(request)真正的代码client.newCall(request).execute()。具体是怎么处理的：

引入一个新类，
1. Realcall：是Call的实现类（目前来看也是唯一实现），call代表着request准备好去执行了，同时也代表一对request和response。
2. getResponseWithInterceptorChain：具体execute的流程，又出现了dispatcher，请求最终前往线程池。在最终执行之前，调用getResponseWithInterceptorChain
这个方法，加入所有之前配置所有系统拦截器，BridgeInterceptor发请求的拦截器.....（拦截器是okhttp很关键的一个概念）
3. RealInterceptorChain：最终执行请求的是RealInterceptorChain。
4. 内部执行的逻辑，就是顾名思义chain链表，第0个拦截器，intercept第1个拦截器生成的chain，第1个拦截器，intercept第2个拦截器生成的chain....以此类推
最后一个chain proceed完了之后，依次往前返回，最终完成请求。（真正在请求的是BridgeInterceptor）

我只查看okhttp最正常的一次请求。以后会不断更新这篇博客。

（水平有限，如果理解的不对，大家一定给我指正）