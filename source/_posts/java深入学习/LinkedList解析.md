---
title: LinkedList解析
date: 2017-02-28 14:52:48
tags:
---
LinkedList与ArrayList一样实现List接口，只是ArrayList是List接口的大小可变数组的实现，
LinkedList是List接口链表的实现。基于链表实现的方式使得LinkedList在插入和删除时更优于ArrayList，
而随机访问则比ArrayList逊色些。

![](../../../../../images/linkedlist/singleLink.jpg)

![](../../../../../images/linkedlist/singleLoopLink.jpg)

![](../../../../../images/linkedlist/doubleLink.jpg)

![](../../../../../images/linkedlist/doubleLoopLink.jpg)

LinkedList 是一个双向循环链表