---
title: android-jni环境搭建
date: 2017-03-09 10:20:21
tags:
---
网上一堆乱七八糟的文章，可以借鉴的并不多。

我来说下我的成功方案吧。

参考 https://codelabs.developers.google.com/codelabs/android-studio-jni/index.html?index=..%2F..%2Findex#0

project gradle

    // Top-level build file where you can add configuration options common to all sub-projects/modules.
    
    buildscript {
        repositories {
            jcenter()
        }
        dependencies {
            classpath 'com.android.tools.build:gradle:2.3.0'
            classpath 'com.android.tools.build:gradle-experimental:0.9.0'
            // NOTE: Do not place your application dependencies here; they belong
            // in the individual module build.gradle files
        }
    }
    
    allprojects {
        repositories {
            jcenter()
        }
    }
    
    task clean(type: Delete) {
        delete rootProject.buildDir
    }

app gradle

    apply plugin: 'com.android.model.application'
    model {
        android {
            compileSdkVersion 25
            buildToolsVersion "25.0.2"
            defaultConfig {
                applicationId "com.zhaokang.ndkapplication"
                minSdkVersion.apiLevel 15
                targetSdkVersion.apiLevel 25
                versionCode 1
                versionName "1.0"
                testInstrumentationRunner "android.support.test.runner.AndroidJUnitRunner"
    
    
            }
            buildTypes {
                release {
                    minifyEnabled false
                    proguardFiles.add(file('proguard-android.txt'))
                }
            }
            ndk {
                moduleName "test"
            }
        }
    }
    dependencies {
        compile fileTree(dir: 'libs', include: ['*.jar'])
        androidTestCompile('com.android.support.test.espresso:espresso-core:2.2.2', {
            exclude group: 'com.android.support', module: 'support-annotations'
        })
        compile 'com.android.support:appcompat-v7:25.2.0'
        compile 'com.android.support.constraint:constraint-layout:1.0.0-beta4'
        compile 'com.android.support:design:25.2.0'
        testCompile 'junit:junit:4.12'
    }
    
这样下来jni项目就搭建好了。