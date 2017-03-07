---
title: jni浅析
date: 2017-03-03 10:51:47
tags:
---
下面给出一张图，通过此图，我们简要说明一下jni是如何连接Java层和本地层的。

![](../../../../../img/jni.jpg).

主要注意的有两点：

1. 静态代码块：


    static {  
           System.loadLibrary("media_jni");  
           native_init();  
       }  


2. native_init的签名：


    private static native final void native_init();  
    
看到静态代码块后，我们可以知道MediaPlayer对应的jni层代码在Media_jni.so库中

本地层对应的so库是libmedia.so，所以MediaPlayer.java通过Media_jni.so和MediaPlayer.cpp(libmedia.so)进行交互

    在Android中，通常java层类和jni层类的名字有如下关系，拿MediaPlayer为例，
    java层叫android.media.MediaPlayer.java，那么jni层叫做android_media_MediaPlayer.cpp
    
在java层代用System.loadLibrary成功后，就会调用jni文件中的JNI_onLoad方法，之中涉及到一个gMethods的变量

    typedef struct {  
        const char* name;  //java层方法名称
        const char* signature;  //方法在签名
        void* fnPtr;  //在jni层对应的函数名称
    } JNINativeMethod; 