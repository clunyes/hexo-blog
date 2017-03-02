---
title: app启动优化
date: 2017-03-02 17:34:36
tags:
---
adb shell am start -W cn.mynewclouedeu/cn.mynewclouedeu.ui.activity.ActivityGuide 
查出启动时间，application初始化使用线程操作。

因为上面这些阶段全部都是在主线程中执行的，任何不经意的操作都可能拖慢应用的启动速度。所以我们不应在Application以及Activity的生命周期回调中做任何费时操作，具体指标大概是你在onCreate，onResume，onStart等回调中所花费的总时间最好不要超过400ms，否则用户在桌面点击你的应用图标后，将感觉到明显的卡顿。但是有些不得以的任务又必须在UI显示之前执行。所以我们要将任务划分优先级。

    优先级为1的在应用启动时，就开始加载

    优先级为2的在首页渲染完成后，开始加载

    优先级为3的在首页渲染完成后，延迟加载 postDelay的方式