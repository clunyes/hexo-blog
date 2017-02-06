---
title: rxjava转换操作符
date: 2017-01-22 14:42:24
tags:
---
##转换操作符
###buffer操作符

buffer操作符周期性地收集源Observable产生的结果到列表中，并把这个列表提交给订阅者，
订阅者处理后，清空buffer列表，同时接收下一次收集的结果并提交给订阅者，周而复始。

###flatMap操作符

flatMap操作符是把Observable产生的结果转换成多个Observable，然后把这多个Observable“扁平化”成一个Observable，并依次提交产生的结果给订阅者。

###concatMap操作符

cancatMap操作符与flatMap操作符类似，都是把Observable产生的结果转换成多个Observable，然后把这多个Observable“扁平化”成一个Observable，并依次提交产生的结果给订阅者。
cancat是有顺序的。

###