---
title: HashMap解析
date: 2017-03-01 16:44:22
tags:
---
   
    要理解HashMap， 就必须要知道了解其底层的实现， 而底层实现里最重要的就是它的数据结构了，
    HashMap实际上是一个“链表散列”的数据结构，即数组和链表的结合体。
    
### 也就是说HashMap的底层结构是一个数组，而数组的元素是一个单向链表。
### 也就是说HashMap的底层结构是一个数组，而数组的元素是一个单向链表。
### 也就是说HashMap的底层结构是一个数组，而数组的元素是一个单向链表。
    
 ----
 小小的插入，位移方法，>>右移（相当于除以2） <<左移（相当于乘以2）
 
     int number = 10;
     System.out.println(Integer.toBinaryString(number));
     number = number >> 1;
     System.out.println(Integer.toBinaryString(number));
     number = number << 3;
     System.out.println(Integer.toBinaryString(number));
    
 ----
    几个关键点：
    
        hashCode的存在主要是用于查找的快捷性，如Hashtable，HashMap等，hashCode是用来在散列存储结构中确定对象的存储地址的；
    
        如果两个对象相同，就是适用于equals(java.lang.Object) 方法，那么这两个对象的hashCode一定要相同；
    
        如果对象的equals方法被重写，那么对象的hashCode也尽量重写，并且产生hashCode使用的对象，
        一定要和equals方法中使用的一致，否则就会违反上面提到的第2点；
    
        两个对象的hashCode相同，并不一定表示两个对象就相同，也就是不一定适用于equals(java.lang.Object) 方法，
        只能够说明这两个对象在散列存储结构中，如Hashtable，他们“存放在同一个篮子里”。
---

threshold初始容量  loadFactor加载因子

    初始容量，加载因子。这两个参数是影响HashMap性能的重要参数，其中容量表示哈希表中桶的数量，
    初始容量是创建哈希表时的容量，加载因子是哈希表在其容量自动增加之前可以达到多满的一种尺度，
    它衡量的是一个散列表的空间的使用程度，负载因子越大表示散列表的装填程度越高
    
