---
title: material design
date: 2016-11-29 14:15:29
tags:
---
   之后要严格遵循material design来开发第二版，关于md所学所想会分享在这里。
   
   - 布局注意点：
   <pre><code>
   1 基线格点
   2 关键线
   3 间距
   4 触摸目标大小
   5 布局结构</code></pre>
   
   - 新特性：
   <pre><code><font size='5'>1. elevation:视图的高度，会影响投影</font>
translationZ:（暂时不知道效果，与elevation共同影响阴影）
阴影参考（https://developer.android.com/training/material/shadows-clipping.html）
<font size='5'>2. 列表，卡片（比较宽大的item）</font>
卡片和列表参考（https://developer.android.com/training/material/lists-cards.html）
<font size='5'>3. material主题</font>
使用material主题方便定制statusbar（需要5.0以上，基于国内情况，目前不支持，可以做个极客项目试试）
参考（https://developer.android.com/training/material/theme.html#Inheritance）
tint重新着色
palette 取色器，从图片中取色
vector 使用xml文件，来绘制icon，优势是占用apk空间小，而且完全适配
<font size='5'>4. material动画</font>
触摸反馈 RippleDrawable
揭露效果 ViewAnimationUtils.createCircularReveal。（就是圆圈变大的效果）
行为转换 Transition 共享元素启动 参考（https://developer.android.com/training/material/animations.html#Reveal）
曲线运动 区别线性运动，针对的是动画Interpolator，前快后慢或者前慢后快
视图动画 StateListAnimator
<font size='5'>5. 兼容性</font>
android5.0以下的兼容办法，参考（https://developer.android.com/training/material/compatibility.html#Layouts）
<font size='5'>6. 动画特性</font>
xml属性 animateLayoutChanges
</code></pre>

- material 追求交互，对交互动画也有要求，动画要<font color='af8888'>快速</font>，要<font color='af8888'>简洁</font>
app内的动画，体验要一致！

我觉得有必要对material design的设计做个目录

单开一个文章[md目录](http://clunyes.github.io/2016/12/1/material-design-目录.md/).

