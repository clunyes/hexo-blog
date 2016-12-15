---
title: volley源码核心
date: 2016-09-20 14:15:08
tags:
---
    今天查看了volley源码，大致看清了volley的处理思路
    
  - Volley这个类，提供了newRequestQueue的方法获得RequestQueue，并启动queue
  - RequestQueue启动时，会关闭之前已开启的NetworkDispatcher和CacheDispatcher（这俩本质就是线程，处理业务），RequestQueue启动第二步，创建新的NetworkDispatcher和CacheDispatcher，并启动这些线程。
  不同的是，CacheDispatcher只会创建一个，NetworkDispatcher会创建4个（线程池size默认是4）。
  - RequestQueue负责接收Request，根据规则Request进入CacheDispatcher队列或者NetworkDispatcher队列（这边规则要说明一下，两者必须选择一个，一般默认是cache的，如果明确要求不要缓存则直接存入network的）
  - CacheDispatcher将轮询自己的队列，如果该request的缓存key查到了，并且有效，那么直接返回数据。反之，将request加入到network的队列中。
  - NetworkDispatcher完成请求后获得NetworkResponse，如果request需要缓存，则缓存下来，同时Delivery回传处理后的response和原始的request。
  - Delivery根据response和request的状态判断，并调用request的响应返回方法deliverResponse，或者deliverError。
  
#### 但是代码中的技巧非常值得学习，很有设计感，volley这份代码很严谨。
  