---
title: jenkins搭建
date: 2017-02-16 17:26:27
tags:
---
需求：持续集成的意义在于，持续将代码集成到主干，优势是快速发现错误，快速迭代。

jenkins怎么搭建

1. 安装jenkins

    下载jenkins war包，java -jar jenkins.war 就可以了
    
2. 安装jenkins插件
    
    在配置，管理插件中，寻找需要的插件，然后安装
    
    针对搭建的iOS/Android持续集成打包平台，我使用到了如下几个插件。
   
       GIT plugin
       SSH Credentials Plugin
       Subversion Plug-in
       Post-Build Script Plug-in：在编译完成后通过执行脚本实现一些额外功能
       Gradle plugin: Android专用
       其余还有好多plugin，plugin都有说明，想用就装

3. 创建项目job
