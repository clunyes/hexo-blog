---
title: ArrayList解析
date: 2017-02-28 14:13:40
tags:
---
ArrayList 是一个数组队列，相当于动态数组。

难点
    
![](../../../../../images/listadd.jpg)

![](../../../../../images/listinsert.jpg)

总结
ArrayList和LinkedList的区别

    ArrayList是实现了基于动态数组的数据结构，LinkedList基于链表的数据结构。
    对于随机访问get和set，ArrayList觉得优于LinkedList，因为LinkedList要移动指针。
    对于新增和删除操作add和remove，LinkedList比较占优势，因为ArrayList要移动数据。

ArrayList和Vector的区别

    Vector和ArrayList几乎是完全相同的,唯一的区别在于Vector是同步类(synchronized)，属于强同步类。因此开销就比ArrayList要大，访问要慢。正常情况下,大多数的Java程序员使用ArrayList而不是Vector,因为同步完全可以由程序员自己来控制。
    Vector每次扩容请求其大小的2倍空间，而ArrayList是1.5倍。
    Vector还有一个子类Stack.
