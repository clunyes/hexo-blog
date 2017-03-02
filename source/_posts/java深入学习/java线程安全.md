---
title: java线程安全
date: 2017-03-01 11:23:00
tags:
---
如何解决线程安全问题？

    基本上所有的并发模式在解决线程安全问题时，都采用“序列化访问临界资源”的方案，即在同一时刻，
    只能有一个线程访问临界资源，也称作同步互斥访问。
    
    可以使用synchronized关键字来标记一个方法或者代码块，当某个线程调用该对象的synchronized方法或者访问synchronized代码块时，
    这个线程便获得了该对象的锁，其他线程暂时无法访问这个方法，只有等待这个方法执行完毕或者代码块执行完毕，
    这个线程才会释放该对象的锁，其他线程才能执行这个方法或者代码块。
    
-----
synchronized的使用

    synchronized代码块，被修饰的代码成为同步语句块，其作用的范围是调用这个代码块的对象，
    我们在用synchronized关键字的时候，能缩小代码段的范围就尽量缩小，能在代码段上加同步就不要再整个方法上加同步。
    这叫减小锁的粒度，使代码更大程度的并发。

    synchronized方法，被修饰的方法成为同步方法，其作用范围是整个方法，作用对象是调用这个方法的对象。

    synchronized静态方法，修饰一个static静态方法，其作用范围是整个静态方法，作用对象是这个类的所有对象。

    synchronized类，其作用范围是Synchronized后面括号括起来的部分synchronized(className.class)，作用的对象是这个类的所有对象。

    synchronized()，()中是锁住的对象， synchronized(this)锁住的只是对象本身，
    同一个类的不同对象调用的synchronized方法并不会被锁住，而synchronized(className.class)实现了全局锁的功能，
    所有这个类的对象调用这个方法都受到锁的影响，此外()中还可以添加一个具体的对象，实现给具体对象加锁。
----
    synchronized (object) {
    //在同步代码块中对对象进行操作
    }
    
----
synchronized注意事项

    当两个并发线程访问同一个对象中的synchronized代码块时，在同一时刻只能有一个线程得到执行，另一个线程受阻塞，必须等待当前线程执行完这个代码块以后才能执行该代码块。两个线程间是互斥的，因为在执行synchronized代码块时会锁定当前的对象，只有执行完该代码块才能释放该对象锁，下一个线程才能执行并锁定该对象。

    当一个线程访问object的一个synchronized(this)同步代码块时，另一个线程仍然可以访问该object中的非synchronized(this)同步代码块。(两个线程使用的是同一个对象)
    当一个线程访问object的一个synchronized(this)同步代码块时，其他线程对object中所有其它synchronized(this)同步代码块的访问将被阻塞(同上，两个线程使用的是同一个对象)。

-----
    当一个线程访问object的一个synchronized(this)同步代码块时，它就获得了这个object的对象锁。
    结果，其它线程对该object对象所有同步代码部分的访问都被暂时阻塞。
    
----
 
    每个类也会有一个锁，它可以用来控制对static数据成员的并发访问。
    并且如果一个线程执行一个对象的非static synchronized方法，另外一个线程需要执行这个对象所属类的static synchronized方法，
    此时不会发生互斥现象，因为访问static synchronized方法占用的是类锁，而访问非static synchronized方法占用的是对象锁，
    所以不存在互斥现象。
    
----
   总结：
   代码块，方法，静态方法都是方法锁（静态方法比较特殊，和对象锁不共用一个锁）
   
   object是对象锁
   
   xx.class 是全局锁
   
----    
面试题

    当一个线程进入一个对象的synchronized方法A之后，其它线程是否可进入此对象的synchronized方法B？
    
    答：不能。其它线程只能访问该对象的非同步方法，同步方法则不能进入。
    因为非静态方法上的synchronized修饰符要求执行方法时要获得对象的锁，如
    果已经进入A方法说明对象锁已经被取走，那么试图进入B方法的线程就只能在等锁池（注意不是等待池哦）中等待对象的锁。

    synchronized关键字的用法？
    
    答：synchronized关键字可以将对象或者方法标记为同步，以实现对对象和方法的互斥访问，可以用synchronized(对象) 
    { … }定义同步代码块，或者在声明方法时将synchronized作为方法的修饰符。

    简述synchronized 和java.util.concurrent.locks.Lock的异同？
    
    答：Lock是Java 5以后引入的新的API，和关键字synchronized相比主要相同点：Lock 能完成synchronized所实现的所有功能；
    主要不同点：Lock有比synchronized更精确的线程语义和更好的性能，而且不强制性的要求一定要获得锁。
    synchronized会自动释放锁，而Lock一定要求程序员手工释放，并且最好在finally 块中释放（这是释放外部资源的最好的地方）