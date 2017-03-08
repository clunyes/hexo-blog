---
title: recycleView
date: 2017-03-08 09:36:08
tags:
---
优点：

    RecyclerView本身它是不关心视图相关的问题的，由于ListView的紧耦合的问题，
    google的改进就是RecyclerView本身不参与任何视图相关的问题。它不关心如何将子View放在合适的位置，
    也不关心如何分割这些子View，更不关心每个子View各自的外观。更进一步来说就是RecyclerView它只负责回收和重用的工作，
    这也是它名字的由来。

    所有关于布局、绘制和其他相关的问题，也就是跟数据展示相关的所有问题，都被委派给了一些”插件化”的类来处理。
    这使得RecyclerView的API变得非常灵活。你需要一个新的布局么？接入另一个LayoutManager就可以了！你想要不同的动画么？
    接入一个新的ItemAnimator就可以了，诸如此类等等。

缺点：

    在RecyclerView中，没有一个onItemClickListener方法。所以目前在适配器中处理这样的事件比较好。
    如果想要从适配器上添加或移除条目，需要明确通知适配器。这与先前的notifyDataSetChanged()方法稍微有些不同。
    具体操作在适配器代码中就可以体现。