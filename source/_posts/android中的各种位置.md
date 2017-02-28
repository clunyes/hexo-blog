---
title: android中的各种位置
date: 2017-02-28 10:57:42
tags:
---
![](../../../../img/androidPosition.png).
    
    View提供的获取坐标方法
    　　getTop()：获取到的是View自身的顶边到其父布局顶边的距离。
    　　getLeft()：获取到的是View自身的左边到其父布局左边的距离。
    　　getRight()：获取到的是View自身的右边到其父布局左边的距离。
    　　getBottom()：获取到的是View自身的底边到其父布局顶边的距离。
----
    MotionEvent提供的方法
    　　getX()：获取点击事件距离控件左边的距离，即视图坐标。
    　　getY()：获取点击事件距离控件顶边的距离，即视图坐标。
    　　getRawX()：获取点击事件距离整个屏幕左边的距离，即绝对坐标。
    　　getRawY()：获取点击事件距离整个屏幕顶边的距离，即绝对坐标。
    　　相信通过上图，读者们应该对MotionEvent和Android坐标系有了一个比较清楚的认识。 