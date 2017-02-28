---
title: java集合理解
date: 2017-02-28 11:13:26
tags:
---
近来面试，屡屡被数据结构算法所虐。

所以最近在恶补这方面的知识，想想以前在学校还是too young too simple，要是有老师傅带带我该多好。
老师我错了，我真的好想再打Dota，哦不，再学数据结构算法。

Java集合是java提供的工具包，包含了常用的数据结构：集合、链表、队列、栈、数组、映射等。

接下来梳理一下
    
    最主要的接口Collection接口，map接口
    
    Collection接口
    
        是List、Set和Queue接口的父接口
        定义了可用于操作List、Set和Queue的方法-增删改查

    List接口

        List是元素有序并且可以重复的集合，被称为序列
        List可以精确的控制每个元素的插入位置，或删除某个位置元素
        List接口的常用子类：
        ArrayList 
        LinkedList 
        Vector
        Stack
    
    Set接口

        Set接口中不能加入重复元素，无序
        Set接口常用子类：
        散列存放：HashSet
        有序存放：TreeSet

    Map和HashMap
    
    Map接口
    
        Map提供了一种映射关系，其中的元素是以键值对（key-value）的形式存储的，能够实现根据key快速查找value
        Map中的键值对以Entry类型的对象实例形式存在
        键（key值）不可重复，value值可以
        每个建最多只能映射到一个值
        Map接口提供了分别返回key值集合、value值集合以及Entry（键值对）集合的方法
        Map支持泛型，形式如：Map<K,V>
    
    HashMap类
    
        HashMap是Map的一个重要实现类，也是最常用，基于哈希表实现
        HashMap中的Entry对象是无序排列的
        Key值和Value值都可以为null,但是一个HashMap只能有一个key值为null的映射（key值不可重复）

    Comparable和Comparator
    
    Comparable接口——可比较的
    
        实现该接口表示：这个类的实例可以比较大小，可以进行自然排序
        定义了默认的比较规则
        其实现类需要实现compareTo()方法
        compareTo()方法返回正数表示大，负数表示小0表示相等
    
    Comparator接口——比较工具接口
    
        用于定义临时比较规则，而不是默认比较规则
        其实现类需要实现compare()方法
        Comparable和Comparator都是Java集合框架的成员
    
    Iterator接口
    
        集合输出的标准操作
        标准做法，使用Iterator接口
        操作原理：
        Iterator是专门的迭代输出接口，迭代输出就是将元素一个个进行判断，判断其是否有内容，如果有内容则把内容取出。

![](../../../../../img/collections.jpg).