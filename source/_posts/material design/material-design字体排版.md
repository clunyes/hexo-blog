---
title: material design字体排版
date: 2016-12-07 14:54:50
tags:
---
- 首先字体分English and English-like、Dense和Tall，就拿我们的app来说English and English-like是英语的书写体，
Dense是中文的书写体
- 还有一个要点 font weight 有个很完美的翻译，字重！noto字重有Thin, Light, DemiLight, Regular, Medium, Bold, and Black
roboto字重有Thin, Light, Regular, Medium, Bold, and Black.由于roboto不支持中文，因此中文app开发者应该使用
noto作为默认字体。[noto下载地址](https://www.google.com/get/noto/) 
- English and English-like scripts ![](../../../../../images/roboto.png)
- Dense scripts ![](../../../../../images/noto.png)
- 行间距问题（以前我一直觉得行间距改不了），其实能改android:lineSpacingExtra用于设置行间距具体值
 android:lineSpacingMultiplier 用于设置默认行间距的倍数
- 这里要额外说明一点，有些字体写了87% black，这里的百分比就是透明度#ffffffff，前两位透明度，
后6位rgb这个大家都明白吧，这里可能要换算下
100% — FF
95% — F2
90% — E6
85% — D9
80% — CC
75% — BF
70% — B3
65% — A6
60% — 99
55% — 8C
50% — 80
45% — 73
40% — 66
35% — 59
30% — 4D
25% — 40
20% — 33
15% — 26
10% — 1A
5% — 0D
0% — 00
给出一张表格，solid指的是100%。