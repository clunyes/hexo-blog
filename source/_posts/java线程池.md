---
title: java线程池
date: 2017-02-28 17:27:56
tags:
---
首先要理清几个类

    Executor 接口，只有一个抽象方法，执行runnable任务。

    ExecutorService 接口，继承自Executor，添加了一些生命周期管理的方法。
    Executor的生命周期有三种状态，运行 ，关闭 ，终止。Executor创建时处于运行状态。
    当调用ExecutorService.shutdown()后，处于关闭状态，isShutdown()方法返回true。
    这时，不应该再想Executor中添加任务，所有已添加的任务执行完毕后，Executor处于终止状态，isTerminated()返回true。
    如果Executor处于关闭状态，往Executor提交任务会抛出unchecked exception RejectedExecutionException。
    
    AbstractExecutorService 抽象类，实现了ExecutorService
    
    ThreadPoolExecutor 实现类，继承自AbstractExecutorService
    
    