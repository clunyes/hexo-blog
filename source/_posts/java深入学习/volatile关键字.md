---
title: volatile关键字
date: 2017-03-01 14:54:16
tags:
---
首先放一张图，不懂不要紧，说实话看了全篇还是有不懂的

![](../../../../images/volatile.jpg).

线程中的三个概念
   
原子性
    
    对基本数据类型的变量的读取和赋值操作是原子性操作，即这些操作是不可被中断的，要么执行，要么不执行。
    
    比较难懂。
    
可见性
    
    对于可见性，Java提供了volatile关键字来保证可见性。
    
    当一个共享变量被volatile修饰时，它会保证修改的值会立即被更新到主存，当有其他线程需要读取时，它会去内存中读取新值。
    
有序性

    在Java内存模型中，允许编译器和处理器对指令进行重排序，
    但是重排序过程不会影响到单线程程序的执行，却会影响到多线程并发执行的正确性。
    
    相当绕。
    
    
看个例子：

    public class VolatileExample extends Thread{
        //设置类静态变量,各线程访问这同一共享变量
        private  static boolean flag = false;
        //无限循环,等待flag变为true时才跳出循环
       public void run() {
           while (!flag){
           };
           System.out.println("停止了");
       }

        public static void main(String[] args) throws Exception {
            new VolatileExample().start();
            //sleep的目的是等待线程启动完毕,也就是说进入run的无限循环体了
            Thread.sleep(100);
            flag = true;
        }
    }


下面解释一下这段代码为何有可能导致无法中断线程。
在前面已经解释过，每个线程在运行过程中都有自己的工作内存，那么线程VolatileExample在运行的时候，
会将flag变量的值拷贝一份放在自己的工作内存当中。

那么当线程main更改了flag变量的值之后，但是还没来得及写入主存当中，线程main转去做其他事情了（这个是关键-立马转去做别的事情了），
那么线程VolatileExample由于不知道线程main对flag变量的更改，因此还会一直循环下去。

-----    
使用volatile后
![](../../../../images/volatile2.jpg).

下面将关键字synchronized和volatile进行一下比较：
1）关键字volatile是线程同步的轻量级实现，所以volatile性能肯定比synchronized要好，并且volatile只能修饰于变量，而synchronized可以修饰方法，以及代码块。随着JDK新版本的发布，synchronized关键字在执行效率上得到很大提升，在开发中使用synchronized关键字的比率还是比较大的。

2）多线程访问volatile不会发生阻塞，而synchronized会出现阻塞。

3）volatile能保证数据的可见性，但不能保证原子性；而synchronized可以保证原子性，也可以间接保证可见性，因为它将私有内存和公共内存中的数据做同步。

4）再次重申一下，关键字volatile解决的是变量在多个线程之间的可见性；而synchronized关键字解决的是多个线程之间访问资源的同步性。

除了synchronize还可以用atomic原子，原子数据类型，操作也是原子的。