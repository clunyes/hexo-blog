---
title: rxjava操作符
date: 2017-01-21 09:42:17
tags:
---
[操作符这里讲解的很仔细](http://blog.csdn.net/job_hesc/article/details/45798307)

RxJava的强大之处，在于它提供了非常丰富且功能强悍的操作符，通过使用和组合这些操作符，你几乎能完成所有你想要完成的任务


- (Observable的创建操作符)，比如：Observable.create()、Observable.just()、Observable.from()等等；
- (Observable的转换操作符)，比如：observable.map()、observable.flatMap()、observable.buffer()等等；
- (Observable的过滤操作符)，比如：observable.filter()、observable.sample()、observable.take()等等；
- (Observable的组合操作符)，比如：observable.join()、observable.merge()、observable.combineLatest()等等；
- (Observable的错误处理操作符)，比如:observable.onErrorResumeNext()、observable.retry()等等；
- (Observable的功能性操作符)，比如：observable.subscribeOn()、observable.observeOn()、observable.delay()等等；
- (Observable的条件操作符)，比如：observable.amb()、observable.contains()、observable.skipUntil()等等；
- (Observable数学运算及聚合操作符)，比如：observable.count()、observable.reduce()、observable.concat()等等；
- 其他如observable.toList()、observable.connect()、observable.publish()等等；

##创建操作符
- 常用的创建操作符还是： create from timer interval（像range just defer真是没用过）
###create操作符

create操作符是所有创建型操作符的“根”，也就是说其他创建型操作符最后都是通过create操作符来创建Observable的

###from操作符

from操作符是把其他类型的对象和数据类型转化成Observable

###just操作符

just操作符也是把其他类型的对象和数据类型转化成Observable，它和from操作符很像，只是方法的参数有所差别

###defer操作符

defer操作符是直到有订阅者订阅时，才通过Observable的工厂方法创建Observable并执行，defer操作符能够保证Observable的状态是最新的

###timer操作符

timer操作符是创建一串连续的数字，产生这些数字的时间间隔是一定的

###interval操作符

interval操作符是每隔一段时间就产生一个数字，这些数字从0开始，一次递增1直至无穷大；interval操作符的实现效果跟上面的timer操作符的第二种情形一样

###range操作符

range操作符是创建一组在从n开始，个数为m的连续数字，比如range(3,10)，就是创建3、4、5…12的一组数字

###repeat/repeatWhen操作符

repeat操作符是对某一个Observable，重复产生多次结果