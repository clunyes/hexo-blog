---
title: '''android多渠道打包'''
date: 2017-05-04 11:11:58
tags:
---
来到新公司，做业务赶项目，所以没来写博客了。

新的公司：我的感觉非常不错，团队氛围，责任心，实事求是。唯一的bug就是加班太狠。自己在这个团队中希望发挥更大的作用。

一直想看下怎么做多渠道打包，昨晚终于静下来去看了，美团的多渠道打包，完美解决了我的问题。

https://github.com/GavinCT/AndroidMultiChannelBuildTool 秒打包。

手动写入读取渠道名，比如友盟


    MobclickAgent.UMAnalyticsConfig config = new MobclickAgent.UMAnalyticsConfig(this, AppConstant.UMENG_APP_KEY, ChannelUtil.getChannel(this, AppConstant.DEFAULT_CHANNEL));


可以读取出渠道名的，已经满足了我的需求。
技术是不停步的，随着7.0的推出，新的签名方式已经出现了，解决方案http://tech.meituan.com/android-apk-v2-signature-scheme.html。不过这个我没看，感觉还用不上。