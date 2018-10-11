---
title: glide源码学习记录
date: 2018-06-08 09:04:12
tags:
---

1. preconditions: 优雅的参数检测，以后项目可以用
2. 单例

        public static Glide get(@NonNull Context context) {
             if (glide == null) {
               synchronized (Glide.class) {
                 if (glide == null) {
                   checkAndInitializeGlide(context);
                 }
               }
             }
            return glide;
          }
3. 泛型使用
源码中大量使用了泛型，以及泛型方法，泛型接口，泛型类

     E - Element (在集合中使用，因为集合中存放的是元素)

     T - Type（Java 类）， S、U、V  - 2nd、3rd、4th types

     K - Key（键）

     V - Value（值）

     N - Number（数值类型）

    ？ -  通配符

4. 分包

        glide的包分为
        load：加载和解码
        manager：生命周期控制
        module
        provider
        request：加载图片的请求
        signature：
        util
        以及默认包
分包比较简练，对外提供简单的接口，比较好懂。内部代码严谨，比较规范。

5. glide的初始化比较简单：

        通过AndroidManifest和@GlideModule注解获取用户自定义配置GlideModule，并调用其对应的方法
        通过GlideBuilder构建Glide：
        1.新建线程池
        2.新建图片缓存池和缓存池
        3.新建内存缓存管理器
        4.新建默认本地缓存管理器
        5.新建请求引擎Engine
        6.新建RequestManger检索器
        7.新建Glide
        Glide构造方法中，新建模型转换器，解码器，转码器，编码器，以及生成Glide上下文GlideContext
        通过RequestManager检索器，建立生命周期监听，并建立一个RequestManager
        完成！

6. into的逻辑，包含了网络请求，调用比较巧妙，使用了桥接模式
7. 大量使用了注解，@NonNull，这个就又可以细讲了，大多数编译注解，作为编译期的辅助工具。这些注解自己项目也可以用起来，这样能避免一些空指针。
8. module的注入