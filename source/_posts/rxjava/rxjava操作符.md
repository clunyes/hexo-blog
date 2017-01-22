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



