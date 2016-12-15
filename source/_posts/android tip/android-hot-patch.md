layout: '[post]'
title: android hot patch
date: 2016-04-28 16:37:32
tags:
---
#### deprecated:androidfix bug实在太多，已经不用这个了。
昨天到今天完成andFix的部署，并且成功使用。
过程大致如下，
# 1
首先当然是集成项目
```python
dependencies {
compile 'com.alipay.euler:andfix:0.4.0@aar'
}
```
# 2
然后加上混淆的参数
```
-dontwarn android.annotation
-dontwarn com.alipay.euler.**
-keep class com.alipay.euler.** {*;}
-keep class * extends java.lang.annotation.Annotation
-keepclasseswithmembernames class * {
    native <methods>;
}
```
# 3
在application中初始化
```
String appversion = UtilDevice.getVersionName();
UtilPatch.getInstance(this).initPatch(appversion);
```
# 4
下载完patch文件之后，addpatch，成功
# 5
patch文件如何生成呢，之后就是打补丁的过程了，首先生成一个apk文件，然后更改代码，在修复bug后生成另一个apk。
通过官方提供的工具apkpatch生成一个.apatch格式的补丁文件，需要提供原apk，修复后的apk，以及一个签名文件。
apkpatch -f fix.apk -t old.apk -o E:\patch -k < key path > -p < password > -a < alias > -e < password >

大约是这样的，目前我还没有加上推送，加上推送效果更好。

目前该项目已经被阿里hotfix替代