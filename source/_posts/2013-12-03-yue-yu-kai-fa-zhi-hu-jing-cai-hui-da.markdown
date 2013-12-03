---
layout: post
title: "越狱开发知乎精彩回答"
date: 2013-12-03 11:12
comments: true
categories: os

---

最近笔者研究越狱开发,现总结了一些资料知乎上他人对越狱开发学习流程的精彩回答.

1.suu，iOS Researcher
<pre>
①去Cydia下载Theos Tutorials，并在谷歌上了解theos相关信息。
②Mac上安装iOSOpenDev，并了解相关信息。
入门教程最多Hello World，剩下的去Github上找源码吧，比如killbackgrounds

我也上传了一个:
http://github.com/al1enSuu/Slide2Dismiss
</pre>

2.季逸超，Peak-Labs创始人/CEO,猛犸浏览器、Rasgue…

<pre>
有幸被邀请回答，不过不知道您要了解的'系统机制'有多深入? ;-)
按照意图和深度的话，大概有这么几种途径与资源：

1.为了学习框架，提升开发水平，可以看看私有API列表。iOS (Cocoa Touch)的各私有API都可以通过runtime查看获得，您可以自己写个method browser。如果觉得麻烦的话可以到Github看现成的，我收藏了俩: https://github.com/kennytm/iphone-private-frameworks 和 https://github.com/nst/iOS-Runtime-Headers ，但还是推荐自己来实时获取，因为iOS在更新，API也在更新。在App Store产品中使用私有API是违反苹果规定的，所以能不用这些API而实现一些功能是iOS工程师水平的体现。

2.对iOS工程师而言，如果只是开发的话(1)也就差不多了。如果您十分有爱，想了解API以下的东西的话，依然可以利用Obj-C的runtime。可以在这里看到 http://opensource.apple.com/source/objc4/objc4-493.11/runtime/ ，尤其是objc-runtime.m，这里提供了很多学习用的"工具"。比如经典的method_exchangeImplementations()，您可以用它研究很多黑箱过程的来龙去脉。值得一提的是，这种技巧(method swizzling)是合法的,可以在App Store 中使用! 苹果曾给使用了相关技巧的开发者发过邮件，表示出于安全性和稳定性最好不再使用，但没有禁止。

3.如果是对系统本身感兴趣的话，不妨越狱看看。iOS和Mac OS X类似，基于Darwin，是一种UNIX系统。越狱后你就有了root权，可以安装个Terminal，装gcc都没问题的哈哈~ 接下来就像您研究Linux那样摆弄就好了。对于开发者来说，有了root权也就可以写一些system tweak或全局的代码，自然也可以用来深入了解系统、原生app等。这方面我很久没折腾了，所以不敢瞎说。

4.如果您是想成为一名iOS Hacker的话，最近有本书挺火的: http://www.amazon.com/iOS-Hackers-Handbook-Charlie-Miller/ 我没空看不知道咋样，但作者很神。另外现在iOS越狱界也有了自己的大会，可以看看“越狱梦之队”的演讲和文档: http://absinthejailbreak.com/dream-team-presentation-at-hitbsecconf-videos/ 。如果您还是没有满足的话，可以看看从硬件入手的逆向工程和调试，分享一个我收藏的宝贝: http://wenku.baidu.com/view/dae22c30eefdc8d376ee32c9.html

5.另外说iOS代码是封闭/闭源的其实不全对，苹果算是开源界的一面大旗了，比如WebKit。iOS的组成部分也一样是开源的，可以在官网 http://opensource.apple.com/ 看到，最新的iOS 5.1.1在这: http://opensource.apple.com/release/ios-511/ 。但是如您所见，这里并没有iOS操作系统的代码，而是一些库和编译器、调试器...其中JavaScriptCore和WebCore很有用，这两者是WebKit的基础，可以说WebKit是iOS最重要的组成之一，截止iOS 5 (6我还没下呢=___=)，所有多于一行文字的控件其实都是WebKit标准的(不可思议吧?!)。很多iOS的Hack都是从这里开始的。说到WebKit,之前Comex大神的Spirit越狱(那个"Slide to Jailbreak")就是利用Safari->WebKit->PDF Engine->TIFF字体的漏洞实现了代码注入！所以每一个系统组件都可能是iOS逆向/Hack的突破口！
</pre>

#参考资料:

1.http://www.zhihu.com/question/20954912 
2.http://www.zhihu.com/question/20317296/answer/14735312