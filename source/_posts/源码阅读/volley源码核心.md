---
title: volley源码核心
date: 2016-09-20 14:15:08
tags:
---
    今天查看了volley源码，大致看清了volley的处理思路
    
  - Volley这个类，提供了newRequestQueue的方法获得RequestQueue，并启动queue
  - RequestQueue启动时，会关闭之前已开启的NetworkDispatcher和CacheDispatcher（这俩本质就是线程，处理业务），RequestQueue启动第二步，创建新的NetworkDispatcher和CacheDispatcher，并启动这些线程。
  不同的是，CacheDispatcher只会创建一个，NetworkDispatcher会创建4个（线程池size默认是4）。
  - RequestQueue负责接收Request，根据规则Request进入CacheDispatcher队列或者NetworkDispatcher队列（这边规则要说明一下，两者必须选择一个，一般默认是cache的，如果明确要求不要缓存则直接存入network的）
  - CacheDispatcher将轮询自己的队列，如果该request的缓存key查到了，并且有效，那么直接返回数据。反之，将request加入到network的队列中。
  - NetworkDispatcher完成请求后获得NetworkResponse，如果request需要缓存，则缓存下来，同时Delivery回传处理后的response和原始的request。
  - Delivery根据response和request的状态判断，并调用request的响应返回方法deliverResponse，或者deliverError。
  
----
以上是之前总结的，现在结合大神的姿势，我再理理顺
  
![](../../../../../images/volley.png)
  
    RequestQueue并不是一个简单的Queue，内部有好几个Queue，其中mCurrentRequests 存放所有请求，
    包括已经运行的和正在等待的请求；mCacheQueue （Request）表示目前缓存的请求队列，
    这个队列会被传入到CacheDispatcher做判断；mNetworkQueue 表示请求网络连接的队列，
    里面的请求都会向服务器进行请求（这才是唯一和服务器打交道的队列）。  

----    
Volley提供的功能

    Json，图像等异步下载
    网络请求的排序（scheduling）
    网络请求的优先级处理
    缓存
    多级别取消请求
    和 Activity 的生命周期联动（Activity 结束时同时取消所有网络请求）
    
volley 流程图

![](../../../../../images/volley_flow.png)

Volley的磁盘缓存

在面试的时候，聊到 Volley 请求到网络的数据缓存。当时说到是 Volley 会将每次通过网络请求到的数据，采用FileOutputStream，写入到本地的文件中。
那么问题来了：这个缓存文件，是声明在一个SD卡文件夹中的(也可以是getCacheFile())。如果不停的请求网络数据，这个缓存文件夹将无限制的增大，最终达到SD卡容量时，会发生无法写入的异常(因为存储空间满了)。
这个问题的确以前没有想到，当时也没说出怎么回事。回家了赶紧又看了看代码才知道，原来 Volley 考虑过这个问题(汗!想想也是)
翻看代码
    
    DiskBasedCache#pruneIfNeeded()

    private void pruneIfNeeded(int neededSpace) {
        if ((mTotalSize + neededSpace) < mMaxCacheSizeInBytes) {
            return;
        }
    
        long before = mTotalSize;
        int prunedFiles = 0;
        long startTime = SystemClock.elapsedRealtime();
    
        Iterator<Map.Entry<String, CacheHeader>> iterator = mEntries.entrySet().iterator();
        while (iterator.hasNext()) {
            Map.Entry<String, CacheHeader> entry = iterator.next();
            CacheHeader e = entry.getValue();
            boolean deleted = getFileForKey(e.key).delete();
            if (deleted) {
                mTotalSize -= e.size;
            } else {
        //print log
            }
            iterator.remove();
            prunedFiles++;
            if ((mTotalSize + neededSpace) < mMaxCacheSizeInBytes * HYSTERESIS_FACTOR) {
                break;
            }
        }
    }
    
其中mMaxCacheSizeInBytes是构造方法传入的一个缓存文件夹的大小，如果不传默认是5M的大小。
通过这个方法可以发现，每当被调用时会传入一个neededSpace，也就是需要申请的磁盘大小(即要新缓存的那个文件所需大小)。
首先会判断如果这个neededSpace申请成功以后是否会超过最大可用容量，如果会超过，
则通过遍历本地已经保存的缓存文件的header(header中包含了缓存文件的缓存有效期、占用大小等信息)去删除文件，
直到可用容量不大于声明的缓存文件夹的大小。
其中HYSTERESIS_FACTOR是一个值为0.9的常量，应该是为了防止误差的存在吧(我猜的)。

Volley缓存命中率的优化

如果让你去设计Volley的缓存功能，你要如何增大它的命中率。
可惜了，如果上面的缓存功能是昨天看的，今天的面试这个问题就能说出来了。
还是上面的代码，在缓存内容可能超过缓存文件夹的大小时，删除的逻辑是直接遍历header删除。
这个时候删除的文件有可能是我们上一次请求时刚刚保存下来的，屁股都还没坐稳呢，现在立即删掉，有点舍不得啊。
如果遍历的时候，判断一下，首先删除超过缓存有效期的(过期缓存)，其次按照LRU算法，删除最久未使用的，岂不是更合适？