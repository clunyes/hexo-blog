---
title: material-design-目录
date: 2016-12-01 14:31:41
tags:
---
### https://material.google.com/地址

### md作为android开发的规范，对于开发、产品、设计都很有意义，我会持续完善这份文档。

## material design 
1. Introduction
2. Environment
3. Material properties 这章说明了md就是各种元素都在自己的层面上，我的理解就是z轴值不一样，这样界面看上去
 就很有层次感
4. Elevation and shadows（高度，阴影）(前三章都在介绍md是啥，这章给出了<font color='af8888'>elevation的标准</font>，非常重要)
5. What's new md是不断更新的，md就像一个协议，不断更新所以是有生命力的（牛逼）

## Motion（手势，动作）
1. Material motion 核心动画要点1.要快，2.要简洁，3.同一个app相同意义的动画要一致
2. Duration and easing 动画自然，曲线动画都是先x轴快，过了中点就是y轴快，
 还有就是要快，不要放慢动画
3. Movement 竖直动画，不要曲线，暗合简洁的概念
 4. Transforming material 变形动画，这章内容涉及的就很细了，关键的一点就是，1做动画的按钮要是圆
 形的，2还有就是矩形扩大缩小的动画，在宽度和高度的变化的时候有先后顺序
 5. Choreography（有道一下：是编舞）讲了一些切换动画的注意点，说实话没太看懂，总而言之，
 做动画不要搞非主流，快而简洁的动画是关键 
 6. Creative customization（创造性定制）部分系统按钮，有两个状态

## Style 
1. Color 这里有几个颜色要注意：
    1. Primary color 主色，会大量使用
    2. Secondary color 用于相关的操作和信息，会比主色深一些或者浅一些，主要看主色是什么色调的
    3. Accent color 用于交互元素或者float action button（fab），接下来就是一些配色，
    接下来看了个light theme和dark theme
2. Icons 图标的制作这个<font color='af8888'>绝壁是给ui看的，希望各位ui能看一下</font>，总结就是不要非主流，要有立体感，
 遵循集合原理。（参考https://material.google.com/style/icons.html#icons-system-icons）
3. Imagery 意象（吐槽下google这几个页面太大了，翻墙很吃力)，适合所有人看的一章，图片该怎样，不该怎么样，有些挺有道理的
4. Typography（排版）  [文字排版单开了一篇文章](http://clunyes.github.io/2016/12/07/material-design字体排版/)
5. Writing 这个很适合我们仔细研读，由于我要开发app的2.0版本，
 [这里我先欠着我会记得的](http://clunyes.github.io/2016/12/1/material-design-writing-产品都应该看/)，
 我也会看的，因为我是半个移动端的产品.

## Layout
1. Principles 提出了seam和step的概念，以及fab放在seam和step上面
2. Units and measurements [屏幕密度，设计开发注意事项，我在这里详细说明了](http://clunyes.github.io/2016/12/07/屏幕密度思考/)
3. Metrics & keylines（度量，基准线）<font color='af8888'>一些边距，包括常用的title listview等等，还有桌面和平板的规范也有，不过我现在只做手机的
 ，比例基准（这个没怎么看太懂），可点击空间的尺寸 touch targets should be at least 48 x 48 dp，Avatar: 40dp Icon: 24dp。
 Touch target on both: 48dp，Touch target height: 48dp，Button height: 36dp这些是硬性规定。规范很重要。</font>
4. Structure 结构 必有的appbar+fab 不必有的bottombar，Side nav menus，其中appbar就是特殊的toolbar，appbar可以展开，
 高度就是56dp+内容高度。statusbar，沉浸式允许隐藏statusbar，点击或者滑动，statusbar显示
5. Responsive UI 这章没看懂。我的理解是屏幕变换的时候，ui的排列，具体到手机是横竖屏转换
6. Split screen 分屏，没看懂

## Components 非常关键的一章，涉及了几乎所有的控件的定义以及规范，开发人员应该严格遵循
1. Bottom navigation 
    1. 对于底部导航数目，item尺寸，颜色都做了说明。
    2. Behavior这个很关键，Navigation through the bottom navigation bar should reset the task state.
 reset就是说，点击item切换之后，列表返回初始状态，不管之前什么状态，点击当前item回到列表最上方。
    3. The bottom navigation bar can appear and disappear dynamically upon scrolling:Scrolling downward hides the bottom navigation bar
Scrolling upward reveals it，<font color='af8888'>上滑bottombar消失，下滑出现</font>。
    4. 结尾处交代了阴影的高度
    
2. Bottom sheets(底部页面)
    1. 分为persistent底部页面，modal底部页面
    2. modal用于分享，persistent一直都是有的，和app内容有比较大的关联度
    3. 最后介绍了具体尺寸规范

3. Buttons
    1. Flat buttons 纯文字的button，文字要大些指的是英文，点击会有墨水效果，但是不会lift，lift应该是elevation变化的效果。
    2. Raised buttons 矩形按钮，点击会有墨水效果，还有lift效果
    3. fab 圆形的按钮，其余和raised button一样 
    4. 这里对button的size有着很明确的说明，以后我会整理一片文章，把size有关的信息单独列出来 
    5. 最后介绍了，dropdown button也就是dropdown list，还有togglebutton
    
4. Buttons: Floating Action Button
    1. fab定义是页面中最常用的按钮有两种size，默认size 56*56，mini size，用于连接页面
    2. 一个页面最多一个fab
    3. fab用于积极的操作，创建，不要用于删除等其他操作 
    4. 后面介绍了fab的使用规范，哪些ok哪些不对
   
5. Cards
    1. cards统一有2dp的corner
    2. item如果没有很多操作的话，不要用cards
    3. cards 的布局可以很多样
    4. For developers android cards/Polymer card
    5. cards支持的操作 swipe，pick up and move（重新排序）
    6. cards内部不要使用滑动，要显示完全或者使用折叠
    7. 各种cards的尺寸，使用cards进行开发，完全可以参照这个文档
    8. 两种分割线的区别，区块分割线（到头），内容分割线（使用padding的）
    
6. Chips small block 小块 这个控件用的比较少的，在手机群发短信的时候见到过
7. Data tables 用于桌面应用，忽略
8. Dialogs 
    1. 类型：1 alert 2 simple menu 3 simple dialog 4 confirmation dialog
    2. 全屏dialog和全屏的bottomsheet完全一样
    3. 打个比方，删除对话框：是否删除该条记录，两个选项文案应该是取消，删除，而不是取消，确定。
    4. 带有title的dialog用于高危的情况，需要用户确实知道情况。
    5. simple dialog不需要cancel按钮，取消按钮总是放在左边
    6. dialog中不要给用户模糊的选项
    7. 最后介绍了dialog的size
9. Dividers 使用的注意事项，没啥特别的重点。
10. Expansion panels 用于创建和轻量的编辑
11. Grid lists 可用于替代标准的listview
    1. 适合做imagegallery，介绍了一些尺寸，其实这种控件不是很常用，两行用cards更适合
    
12. Lists
    1. 超过两行的文案，建议使用cards
    2. 把最重要的内容放在左面，把最不重要的内容放在右边
    3. 各种list的size，list开发界面规范见此
    
13. Lists: Controls list上的操作
    1. checkbox可以是主action，可以是辅action
    2. Switch（开关），Reorder（重新，排序），Expand/collapse（展开收回）是辅action
    3. Leave-behinds，隐藏操作，通过从左向右滑动item 或者从右向左滑动item，出现操作
    4. menu是一类特别的list，操作规则适用于dropdown menus, overflow menus，和标准的list不一样
    
14. Menus 一个暂时的选择列表，开发一般用popupwindow来实现
    1. menu必须在所有ui之上，而且要遮住当前要编辑的内容（这个和目前国内大部分app的设计
    有些不一样）
    2. 使用simple menu来实现（见dialog那一章，提到了simple menu）
    3. menu弹出的位置很有讲究，具体看配图
    4. 最后照例是尺寸说明
    
15. Pickers 主要讲的就是date picker和timer picker，用系统自带的就行
16. Progress & activity 这里指的是进度指示
    1. 进度包括：确定的、不确定的、buffer缓冲的、不确定查询返回确定的值，常用的场景就是前3种
    2. 包括线条装loading，圆形loading
    3. 分阶段加载：容器先出来，内容再出来，不需要太细究
    
17. Selection controls 用于选择的操作控件
    1. Checkbox 多选框
    2. Radio button 单选框 
    3. Switch 开关  
    4. 状态为hover, focused, pressed, disabled, and disabled focused states.
    
18. Sliders 滑动条
    1. 持续滑块，可以连续滑动
    2. 离散滑块（每一单元值是固定，单元是滑动最小单位）
    
19. Snackbars & toasts 都是简单用户操作反馈
    1. snackbar可以点掉，toast不行
    2. snackbar不要挡住fab，把fab顶上去，snackbar不要加图标，尺寸和用法都在配图中说明了。
20. Steppers 很实用的控件，用于表示一系列操作的进度，<font color='af8888'>特别适合用于填复杂
表格，这样的需求，也很适合新版app的测验</font>
    1. 部分ui是tablet的ui，关心mobile就行，最后是尺寸说明
21. Subheaders 子标题，看下尺寸说明就好
22. Tabs 顶部类目分类
    1. tab类目要同一等级，tab类目不要有子类目
    2. tab页名字过长，可以两行，两行过长才用省略号表示
    3. 最后是尺寸
23. Text fields 输入文案的控件
    1. 输入type：__Number__: Phone number, credit card number, PIN；__Text__: Proper name, username, URL
    ；__Mixed format__: Email address, street address, search query
    2. 这边用的就是Textinputlayout
    3. Single-line text field，尺寸说明
    4. Multi-line text field，尺寸说明，超出行的最大限制，自动跳到下一行
    5. Full-width text field，用于深入的书写
    6. 字数统计Character counter，监控字数用
    7. 自动补全和搜索过滤
    8. 最后介绍密码输入的各个情况,__比较实用__
24. Toolbars 没看懂
25. Tooltips 对于图标的功能说明
    1. 长按1.5秒之后，弹出功能说明，比如最常用的optionmenu，item长按会出现说明
26. Widgets 这里的widget指的是桌面小控件，暂时不涉及，而且没怎么看懂
## Patterns 模式，一些固定的模式套路，一类特定的问题
1. Confirmation and acknowledgement 证实确认会减少用户的疑虑，snackbar来完成这类任务
2. Data formats 一般用于日期时间，格式化，文案太长的，或者隐藏部分文案使用ellipses省略号
3. Errors 
    1. 如何处理error：明确告诉用户发生了什么，告诉用户如何解决，保存用户的输入（问题来了
    都知道这样有问题，为啥不去处理，这里有问题）。
    2. 输入的问题，和上面的23节一样，输入错误的提示
    3. 同步异常，给用户再次连接的按钮
    4. 网络异常，提示网络异常
4. Fingerprint 指纹，主要用于解锁和支付，登录注册，还有就是硬件支持指纹
极客学院找到中文版
5. Gestures 手势  http://wiki.jikexueyuan.com/project/material-design/patterns/gestures.html 和英文原版一模一样
6. Launch screens 启动页 http://wiki.jikexueyuan.com/project/material-design/patterns/launch-screens.html
7. Loading images 如何取加载图片
8. Navigation 文章译文没有，讲了导航需要让用户清楚知晓，产品可以仔细看。
整理了一些概念，引入Hierarchy（层级），返回作为一种导航方式特地介绍。
9. Navigation drawer 其实之前list介绍时，已经介绍了，受到ios的影响，国内并不流行抽屉式的菜单。
这章主要介绍尺寸。
10. Navigational transitions 导航动画
    1. 父view到子view
    2. 兄弟view切换，重点在于elevation，父到子elevation要有变化，体现层级不同，子到子elevation
    不变
11. Notifications 消息，一般和推送结合起来使用
    1. 有事务的
    有事务的包括：1.人与人交互的：短信电话 2.日常生活：闹钟 3. 修改某些状态用：暂停歌曲
    2. 没有事务的
    没有事务的包括 1. Opt-out 2. Opt-in ,用户可以通过设置，不接受opt-out的信息
    3. 消息的创建应结合priority（优先级）和categories，具体的值和对应的意义，配图有说明
    做到推送的时候，应该仔细阅读。
12. Permissions 权限
    1. 现在的权限都是，获取具体数据时获取的，不同于以往安装就获取权限。
    2. 重要的权限在应用第一次开启时询问
    3. 不重要的权限到具体使用时询问
    4. 如果用户拒绝了权限，下次操作要提示用户去设置里修改权限
    5. 如果涉及到核心权限被拒绝app无法正常运行，只能不让用户进入app
13. Scrolling techniques 滑动操作 http://wiki.jikexueyuan.com/project/material-design/patterns/scrolling-techniques.html
14. Search 搜索，这个真没什么好说，做到了搜索可以参考下，之前Text fields 也有提到的
15. Selection 这个一般在webview中比较常见，长按选择文案，进行后续操作，暂时没实现过，就不多说。
16. Settings 让用户定义app应该做哪些事，定义了哪些do哪些don't，开发到设置时，可以对比文档
17. Swipe to refresh 下拉刷新，和列表结合使用

## Growth & communications 成长沟通，指的是培养用户
1. Onboarding 培训 说实话，这章对产品的体验而言很重要，但是需要投入大量的工作在其中。暂时不考虑
2. Feature discovery 用户引导，和上一节一样，重要体验好，但是暂时不考虑
3. Gesture education 效果目的很好，怎么实现是个问题

## Usability 可用性
1. Accessibility http://wiki.jikexueyuan.com/project/material-design/usability/accessibility.html 
译文和原文一样
2. Bidirectionality http://wiki.jikexueyuan.com/project/material-design/usability/bidirectionality.html
译文和原文一样

最后一章资源，提供了下载。

