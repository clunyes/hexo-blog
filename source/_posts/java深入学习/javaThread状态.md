---
title: javaThread状态
date: 2017-02-27 10:51:02
tags:
---
首先一个小问题：线程实现可以是thread和runnable，本身两者并没有优劣，但是一个是继承一个是接口，如果你要继承其他类，还是用runnable。

run和start的区别，start方法开启了新线程，run方法没有，start方法不阻塞主线程。

以下是thread的两张状态图，有助于理解线程的生命周期（哪个顺眼你看哪个）。

![](../../../../../img/threadStatus.jpg)

![](../../../../../img/threadProcess.jpg)

两个图有个冲突，即join方法，待验证。

1.     程序通过Thread t = new Thread()，调用t.start()启动一个线程，使该线程进入可运行(Runnable)的状态。

2.     由JVM的决定去调度(Scheduler) 在可运行状态（Runnable）下的线程,使该线程处于运行 (Running) 状态,
       由于JVM的调度会出现不可控性，即不是优先级高的先被调用，可能先调用，也可能后调用的的情况。运行状态(Running)下，
       调用礼让yield()方法，可以使线程回到可运行状态(Runnable)下，再次JVM的调度（并不依赖优先级）。

3.     线程在Running的过程中可能会遇到阻塞(Blocked)情况

①．调用join()和sleep()方法，sleep()时间结束或被打断，join()中断,IO完成都会回到Runnable状态，等待JVM的调度。

②．调用wait()，使该线程处于等待池(wait blocked pool),直到notify()/notifyAll()，线程被唤醒被放到锁池(lock blocked pool )
，释放同步锁使线程回到可运行状态（Runnable）

③．对Running状态的线程加同步锁(Synchronized)使其进入(lock blocked pool ),同步锁被释放进入可运行状态(Runnable)。

4.       线程run()运行结束或异常退出，线程到达死亡状态(Dead)

sleep和wait的区别有：

        sleep是Thread类的方法,wait是Object类中定义的方法.
        
        Thread.sleep不会导致锁行为的改变, 如果当前线程是拥有锁的, 那么Thread.sleep不会让线程释放锁.
        
        Thread.sleep和Object.wait都会暂停当前的线程. OS会将执行时间分配给其它线程. 区别是, 调用wait后, 
        需要别的线程执行notify/notifyAll才能够重新获得CPU执行时间.
        
        
sleep不会释放锁，yield同样不会释放锁。

    join()方法

    在很多情况下，主线程创建并启动了线程，如果子线程中进行大量耗时运算，主线程往往将早于子线程结束之前结束。
    这时，如果主线程想等待子线程执行完成之后再结束，比如子线程处理一个数据，主线程要取得这个数据中的值，就要用到join()方法了。
    方法join()的作用是等待线程对象销毁。
        
线程的上下文切换：

    对于单核CPU来说（对于多核CPU，此处就理解为一个核），CPU在一个时刻只能运行一个线程，
    当在运行一个线程的过程中转去运行另外一个线程，这个叫做线程上下文切换（对于进程也是类似）。
    实际上就是 存储和恢复CPU状态的过程，它使得线程执行能够从中断点恢复执行。

停止线程

停止线程是在多线程开发时很重要的技术点，掌握此技术可以对线程的停止进行有效的处理。
停止一个线程可以使用Thread.stop()方法，但最好不用它。该方法是不安全的，已被弃用。
在Java中有以下3种方法可以终止正在运行的线程：

    使用退出标志，使线程正常退出，也就是当run方法完成后线程终止
   
    使用stop方法强行终止线程，但是不推荐使用这个方法，因为stop和suspend及resume一样，都是作废过期的方法，使用他们可能产生不可预料的结果。
    
    使用interrupt方法中断线程，但这个不会终止一个正在运行的线程，还需要加入一个判断才可以完成线程的停止。

暂停线程

    interrupt()方法
    
线程的优先级

    public final static int MIN_PRIORITY = 1;
    public final static int NORM_PRIORITY = 5;
    public final static int MAX_PRIORITY = 10;
    
    
守护线程
    
    在Java线程中有两种线程，一种是User Thread（用户线程），另一种是Daemon Thread(守护线程)。
    
    Daemon的作用是为其他线程的运行提供服务，比如说GC线程。
    其实User Thread线程和Daemon Thread守护线程本质上来说去没啥区别的，唯一的区别之处就在虚拟机的离开：
    如果User Thread全部撤离，那么Daemon Thread也就没啥线程好服务的了，所以虚拟机也就退出了。

    