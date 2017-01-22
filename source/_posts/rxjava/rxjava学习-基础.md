---
title: rxjava学习-基础
date: 2016-12-12 15:45:40
tags:
---
##Rxjava 使用笔记
最近使用新的框架进行开发，其中最为困惑的就是rxjava，确实入门比较难。
经过反复的更改，最终改名rxjava学习-基础，确实需要仔细理解。

###1.20 
今天终于把2.0项目上线了，那么就系统学习下rxjava，[学习资料在这里](https://github.com/yuxingxin/RxJava-Essentials-CN)
我直接贴过来了，下面的介绍说的很明白
- 响应式编程是一种基于异步数据流概念的编程模式。数据流就像一条河：它可以被观测，被过滤，被操作，或者为新的消费者与另外一条流合并为一条新的流。
- 响应式编程的一个关键概念是事件。事件可以被等待，可以触发过程，也可以触发其它事件。事件是唯一的以合适的方式将我们的现实世界映射到我们的软件中：
如果屋里太热了我们就打开一扇窗户。同样的，当我们更改电子表（变化的传播）中的一些数值时，我们需要更新整个表格或者我们的机器人碰到墙时会转弯（响应事件）。
- 今天，响应式编程最通用的一个场景是UI：我们的移动App必须做出对网络调用、用户触摸输入和系统弹框的响应。在这个世界上，软件之所以是事件驱动并响应的是因为现实生活也是如此。

rx特点

1. 一致性
2. 可扩展：这个在编码过程中特别有感觉。只要思路够清晰，一句话就能把业务处理完，不管同步异步都没问题。
3. 陈述式
4. 可组合
5. 可转化

rx的两个最核心的接口
IObserver 和 IObservable，所有的链式都衍生自这两个接口。代码如下：
```android
//Defines a provider for push-based notification.
public interface IObservable<out T> 
{
//Notifies the provider that an observer is to receive notifications.
IDisposable Subscribe(IObserver<T> observer);   
}
```    
```android
public interface IObserver<in T>
{
//Provides the observer with new data.
void OnNext(T value);
//Notifies the observer that the provider has experienced an error condition.
void OnError(Exception error);
//Notifies the observer that the provider has finished sending push-based notifications.
void OnCompleted();
}
```   
四种角色
- Observable
- Observer
- Subscriber
- Subjects

Observables和Subjects是两个compose的实体，Observers和Subscribers是两个consume实体。

subject是一个神奇的对象，它可以是一个Observable同时也可以是一个Observer：它作为连接这两个世界的一座桥梁。一个Subject可以订阅一个Observable，
就像一个观察者，并且它可以执行新的数据，或者传递它接受到的数据，就像一个Observable。很明显，作为一个Observable，观察者们或者其它Subject都可以订阅它。
一旦Subject订阅了Observable，它将会触发Observable开始执行。如果原始的Observable是“冷”的，这将会对订阅一个“热”的Observable变量产生影响。


RxJava提供四种不同的Subject：

- PublishSubject
- BehaviorSubject
- ReplaySubject
- AsyncSubject

<font color='af8888'>说实话subject我在开发中没怎么注意到，需要到实战中再理解</font>

BehaviorSubject:
简单的说，BehaviorSubject会首先向他的订阅者发送截至订阅前最新的一个数据对象（或初始值）,然后正常发送订阅后的数据流。

ReplaySubject:
ReplaySubject会缓存它所订阅的所有数据,向任意一个订阅它的观察者重发。

AsyncSubject
当Observable完成时AsyncSubject只会发布最后一个数据给已经订阅的每一个观察者。

(这些subject都带有比较特殊的能力，在特定的业务场景下会有优势)



