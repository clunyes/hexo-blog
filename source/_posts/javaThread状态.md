---
title: javaThread状态
date: 2017-02-27 10:51:02
tags:
---
首先一个小问题：线程实现可以是thread和runnable，本身两者并没有优劣，但是一个是继承一个是接口，如果你要继承其他类，还是用runnable。

run和start的区别，start方法开启了新线程，run方法没有，start方法不阻塞主线程。

以下是thread的状态图，有助于理解线程的生命周期。

![](../../../../img/threadStatus.jpg).


1.     程序通过Thread t = new Thread()，调用t.start()启动一个线程，使该线程进入可运行(Runnable)的状态。

2.     由JVM的决定去调度(Scheduler) 在可运行状态（Runnable）下的线程,使该线程处于运行 (Running) 状态,
       由于JVM的调度会出现不可控性，即不是优先级高的先被调用，可能先调用，也可能后调用的的情况。运行状态(Running)下，
       调用礼让yield()方法，可以使线程回到可运行状态(Runnable)下，再次JVM的调度（并不依赖优先级）。

3.     线程在Running的过程中可能会遇到阻塞(Blocked)情况

①．调用join()和sleep()方法，sleep()时间结束或被打断，join()中断,IO完成都会回到Runnable状态，等待JVM的调度。

②．调用wait()，使该线程处于等待池(wait blocked pool),直到notify()/notifyAll()，线程被唤醒被放到锁池(lock blocked pool )，释放同步锁使线程回到可运行状态（Runnable）

③．对Running状态的线程加同步锁(Synchronized)使其进入(lock blocked pool ),同步锁被释放进入可运行状态(Runnable)。

4.       线程run()运行结束或异常退出，线程到达死亡状态(Dead)

sleep和wait的区别有：
1，类：这两个方法来自不同的类分别是Thread和Object
2，锁：最主要是sleep方法没有释放锁，而wait方法释放了锁，使得其他线程可以使用同步控制块或者方法。
3，域：wait，notify和notifyAll只能在同步控制方法或者同步控制块里面使用，而sleep可以在
    任何地方使用
   synchronized(x){
x.notify()
//或者wait()
}
4，异：sleep必须捕获异常，而wait，notify和notifyAll不需要捕获异常

5，停：其实两者都可以让线程暂停一段时间,但是本质的区别是一个线程的运行状态控制,一个是线程之间的通讯的问题

notify():唤醒一个处于等待状态的线程，注意的是在调用此方法的时候，并不能确切的唤醒某一个等待状态的线程，而是由JVM确定唤醒哪个线程，而且不是按优先级。

notifyAll():唤醒所有处入等待状态的线程，注意并不是给所有唤醒线程一个对象的锁，而是让它们竞争。