---
title: android热修复原理
date: 2017-02-22 17:42:36
tags:
---
目前常用的热修复技术

    AndFix   采用的是Native hook方案
    
    Nuwa, HotFix, RocooFix  采用的是QQ空间提出的Classloader替换类的方案
    
    Tinker  采用的是Instant Run的冷插拔原理的Dex替换


以下一一分析

    native hook：  在native层进行指针替换，AndFix基于修复包和原生包的区别，加入注解，
    通过这份注解来找到要替换的方法，如果可以就会hook该方法并进行替换。

-----
    
    Nuwa Hotfix RocooFix这些方案是基于dex分包方案
----- 
    什么是dex分包：
    
    当 Android 系统安装一个应用的时候，有一步是对 Dex 进行优化，这个过程有一个专门的工具来处理，叫 DexOpt。DexOpt 
    是在第一次加载 Dex 文件的时候执行的。这个过程会生成一个 ODEX 文件，即 Optimised Dex。
    执行 ODEX 的效率会比直接执行 Dex 文件的效率要高很多。
    
    但是在早期的 Android 系统中，DexOpt 有两个问题。
    （一）：DexOpt 会把每一个类的方法 id 检索起来，存在一个链表结构里面，但是这个链表的长度是用一个 short 类型来保存的，
    导致了方法 id 的数目不能够超过65536个。一个dex文件最多只支持65536个方法。
    （官方提出了multidex的解决方案）
    
    （二）：Dexopt 使用 LinearAlloc 来存储应用的方法信息。Dalvik LinearAlloc 是一个固定大小的缓冲区。
    在Android 版本的历史上，LinearAlloc 分别经历了4M/5M/8M/16M限制。Android 2.2和2.3的缓冲区只有5MB，
    Android 4.x提高到了8MB 或16MB。当方法数量过多导致超出缓冲区大小时，也会造成dexopt崩溃。
-----
    由于上述问题的存在，通过不断研究，便有了dex分包的解决方案。
-----
    简单来说，其原理是将编译好的class文件拆分打包成两个dex，绕过dex方法数量的限制以及安装时的检查，在运行时再动态加载第二个dex文件中。
    原则上两个dex的类是没有重复的 ，如果classes.dex和classes1.dex中有重复的类，当用到这个重复的类的时候，系统会选择哪个类进行加载呢？
    
    
理论知识来了：
    一个ClassLoader可以包含多个dex文件，每个dex文件是一个Element，多个dex文件排列成一个有序的数组dexElements，
当找类的时候，会按顺序遍历dex文件，然后从当前遍历的dex文件中找类，如果找类则返回，如果找不到从下一个dex文件继续查找。

那么，如果有重复的类，以前面的dex为准。把有问题的类打包到一个dex（patch.dex）中去，然后把这个dex插入到Elements的最前面，就能实现热修复了。

理论到此为止。

----
    Tinker的原理：Instant Run的冷插拔，完全使用了新的Dex，但是又不能直接替换dex，那么怎么做呢。
    
    在编译时通过新旧两个Dex生成差异patch.dex。在运行时，将差异patch.dex重新跟原始安装包的旧Dex还原为新的Dex。
    这里就涉及到怎么最优算patch包的问题。
    
    这套方案有两个缺点：占用更多空间。
              
    一个额外的合成过程；虽然我们单独放在一个进程上处理，但是合成时间的长短与内存消耗也会影响最终的成功率
    (与修改Dex大小、补丁大小相关)。
                  
但是我觉得这两个缺点都不是问题，毕竟热修复不能当做更新来用。。