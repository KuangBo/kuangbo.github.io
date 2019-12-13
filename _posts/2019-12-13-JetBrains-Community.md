---
layout:     post
title:      JetBrains Community
subtitle:   PixelLink:通过实例分割进行场景文本检测
date:       2019-12-13
author:     kuangbo
header-img: img/KotlinConfKeynote2019.png
catalog: true
tags:
    - 深度学习
    - 文本检测
---


## JetBrains

[JetBrains](https://www.jetbrains.com/)（以前称为IntelliJ Software）成立于2000年，是一家捷克的软件开发公司，该公司位于捷克的布拉格，并在俄罗斯的圣彼得堡及美国麻州波士顿都设有办公室，该公司最为人所熟知的产品是Java编程语言开发撰写时所用的集成开发环境：[IntelliJ IDEA](https://www.jetbrains.com/idea/)。

Jetbrains为Java，Kotlin，Ruby，Python，PHP，C，Objective-C，C ++，C＃，Go，JavaScript和SQL编程语言提供了扩展的集成开发环境（IDE）系列。截至2017年6月，该公司共发布了24款开发工具与及相关产品。


###  Products
#### IDEs
<div align="center">
![JetBrains](https://kuangbo.github.io/img/JetBrains.png)
</div>

- AppCode - Swift 和 Objective-C IDE开发工具。
- [CLion](https://www.jetbrains.com/clion/) - 跨平台的C/C++ IDE 开发工具，支持C++11 、C++14、libc++以及Boost。
- DataGrip - 一款数据库客户端工具。
- GoLand - Go语言的集成开发环境。
- [IntelliJ IDEA](https://www.jetbrains.com/idea/) - 2001年发布。一套智能的 Java 集成开发环境，特别专注与强调程序师的开发撰写效率提升。
- PhpStorm - PHP IDE开发工具。
- PyCharm - 一款结合了Django框架的Python IDE开发工具。
- Rider - 一款快速，功能强大，跨平台的.NET IDE开发工具。
- RubyMine - 一套强大的Ruby on Rails IDE开发工具。
- WebStorm - JavaScript的开发工具。

#### Programming languages

- [Kotlin](https://www.jetbrains.com/idea/) - 基于JVM的现代编程语言。Kotlin是一门静态类型、面向对象，旨在避免由Java的向后兼容性引起的问题的编程语言。JetBrains公司于2010年开发出该编程语言并于2012年2月宣布将其开源。Kotlin语言最初设计的目的是替代Java语言。
- JetBrains MPS - MPS给编程语言带来了很大的灵活性。与具有严格的语法和语义限制的传统编程语言不同，MPS允许用户创建新的编程语言，更改或扩展现有的编程语言。


### Open Source Program

[JetBrains开源项目](https://www.diycode.cc/developers/JetBrains)多达20个，其中包括[Kotlin](https://github.com/JetBrains/kotlin)、[intellij-community](https://github.com/JetBrains/intellij-community)、Kotlin-native等。下面着重介绍一下来自JetBrains的Kotlin与IntelliJ IDEA Community。

#### Kotlin
<div align="center">
![KC19_Global_badge_Blogpost](https://kuangbo.github.io/img/KC19_Global_badge_Blogpost.png)
</div>

[Kotlin](https://www.jetbrains.com/idea/)是一种在Java虚拟机上运行的静态类型编程语言，它也可以被编译成为JavaScript源代码。它主要是由俄罗斯圣彼得堡的JetBrains开发团队所发展出来的编程语言，其名称来自于圣彼得堡附近的科特林岛。
Kotlin被设计为一种工业级的面向对象的语言，并且是比Java更好的语言，但仍可与Java代码完全互操作，从而使公司可以逐步从Java迁移到Kotlin。<br>
2019年5月7日，谷歌宣布Kotlin是Android应用程序开发的[首选语言](https://techcrunch.com/2019/05/07/kotlin-is-now-googles-preferred-language-for-android-app-development/)。
<div align="center">
![KotlinConfKeynote2019](https://kuangbo.github.io/img/KotlinConfKeynote2019.png)
</div>

"Our vision is for Kotlin to be a reliable companion for all your endeavors, a default language choice for your tasks. To accomplish this, we’re going to make it shine on all platforms. Multiple case studies from companies well-known in the industry show that we are making good progress in this direction."<br>
"Kotlin 1.4 that is going to arrive in spring 2020 will make another step forward for the Kotlin ecosystem."

#### IntelliJ IDEA Community
<div align="center">
![IDEA](https://kuangbo.github.io/img/idea_overview_5_1.png)
</div>

最初版于2001年1月时推出，当时是少数使用前阶代码浏览及代码重构的Java集成开发环境之一。<br>
在2010年的Infoworld报告中，比较当时市面上的主流Java集成开发环境，包括：Eclipse、IntelliJ、NetBeans、JDeveloper，IntelliJ获得该媒体实测中的最高评比。<br>
2014年12月，Google宣布其旗下专用于发展Android操作系统的首版Android Studio，即基于IntelliJ IDEA的社区版本发展而成，用以取代原来提供Android开发者使用的Eclipse ADT。开发者除了可直接下载Android Studio外，原IntelliJ用户亦可下载其相关插件来进行开发程序。<br>
IntelliJ对个别编程语言所开发的集成环境，如AppCode、CLion、PhpStorm、PyCharm、RubyMine、WebStorm和MPS等，都可以通过插件的方式加载IntelliJ IDEA来使用。

IntelliJ IDEA的各个方面都经过专门设计，以最大程度地提高开发人员的生产力。
强大的静态代码分析和人体工程学设计共同使开发工作不仅富有成效，而且还带来令人愉悦的体验。

- **深度智慧**
IntelliJ IDEA为您的源代码建立索引之后，它会通过在每个上下文中提供相关建议来提供快速，智能的体验，即时而智能的代码完成、即时的代码分析和可靠的重构工具。
- **开箱即用的体验**
关键任务工具，如集成的版本控制系统和各种受支持的语言和框架都是现成的，没有插件的麻烦。
- **智能代码完成**
基本补全建议类名、方法名、字段名和可见性范围内的关键字名，而智能补全只建议当前上下文中需要的类型。
- **特定于框架的协助**
IntelliJ IDEA是一个面向Java的IDE，它也可以理解并为其他各种语言提供智能编码支持，比如SQL、JPQL、HTML、JavaScript等，即使语言表达式被注入到Java代码的字符串文本中。
- **生产力助推器**
IDE可以预测开发者的需求，并自动执行冗长而重复的开发任务，因此开发者可以专注于全局。
- **开发人员人体工程学**
在IDE做出的每个设计和实现决策中，都着重考虑中断开发人员流程的风险，并尽力消除或最小化该流程。
IDE会根据开发者的上下文自动调出相应的工具。
- **不打扰的智能**
IntelliJ IDEA的编码帮助不仅仅是编辑器:它还能帮助开发者在处理其他方面的事情时保持高效率:比如填写一个字段，搜索一个元素列表，访问工具窗口或切换设置等。
