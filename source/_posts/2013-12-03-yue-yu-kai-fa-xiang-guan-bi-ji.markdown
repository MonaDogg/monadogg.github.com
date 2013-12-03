---
layout: post
title: "越狱开发相关笔记"
date: 2013-12-03 11:12
comments: true
categories: os

---

##1安装iOS sdk
Step1:安装iOS SDK.
##2下载theos
可以简单理解theos为越狱开发的环境
<pre>
export THEOS=/opt/theos
//设置环境变量
svn co http://svn.howett.net/svn/theos/trunk $THEOS
//svn下载相关内容到位置
</pre>

可以使用下列语句打印看看:
<pre>
echo $THEOS
</pre>
##3下载ldid
ldid的功能是给app签名
下载并解压到桌面上: http://cloud.github.com/downloads/rpetrich/ldid/ldid.zip

<pre>
chmod +x ~/Desktop/ldid
//设置ldid可以执行的权限

mv ~/Desktop/ldid $THEOS/bin/ldid
//将ldid移动到指定的位置
</pre>

##4安装MacPorts和dpkg
1.先安装Macports,选择合适的系统版本,有可能会卡在最后一分钟，需要重启后断网安装即可。

2.dpkg的作用是将你的app打包为debian paceage.
sudo port install dpkg

##5创建一个新的项目
theos使用一个叫做nic(new instance tool)的工具来创建新的工程。执行下面的命令：
$THEOS/bin/nic.pl
就可以开始创建。

下面是一个创建jailbroken 应用程序的例子：
<pre>
author$ $THEOS/bin/nic.pl
NIC 1.0 - New Instance Creator
——————————
  [1.] iphone/application
  [2.] iphone/library
  [3.] iphone/preference_bundle
  [4.] iphone/tool
  [5.] iphone/tweak
Choose a Template (required): 1
Project Name (required): firstdemo
Package Name [com.yourcompany.firstdemo]: 
Author/Maintainer Name [Author Name]: 
Instantiating iphone/application in firstdemo/…
Done.
</pre>
这样就会在当前目录下创建一个名为fistdemo的文件夹
##6生成文件说明
1.control: 包含applicaton/tweak的信息，当你从Cydia安装时，你可以看到这些信息，包括名字，作者，版本，等等。

2.main.m，程序启动的入口.

3.firstDemoApplication:app的代理文件类

4.Makefile：包含必要的编译命令

5.Resources：包含info.plist文件等

6.RootViewController.h/mm :根vc
其中Makefile的内容为:
<pre>
include theos/makefiles/common.mk

APPLICATION_NAME = firstDemo
firstDemo_FILES = main.m firstDemoApplication.mm RootViewController.mm
firstDemo_FRAMEWORKS = UIKit CoreGraphics

include $(THEOS_MAKE_PATH)/application.mk
</pre>
##7重新设置环境变量
设置下列环境变量:环境位置,sdk版本,设备ip地址.
<pre>
export THEOS=/opt/theos/
export SDKVERSION=7.0
export THEOS_DEVICE_IP=xxx.xxx.xxx.xxx
</pre>
##8构建工程
1.make
<pre>
$ make
Making all for application firstdemo…
 Compiling main.m…
 Compiling firstdemoApplication.mm…
 Compiling RootViewController.mm…
 Linking application firstdemo…
 Stripping firstdemo…
 Signing firstdemo…
</pre> 
2.make package
<pre>
make package
Making all for application firstdemo…
make[2]: Nothing to be done for ‘internal-application-compile’.
Making stage for application firstdemo…
 Copying resource directories into the application wrapper…
dpkg-deb: building package ‘com.yourcompany.firstdemo’ in ‘/Users/author/Desktop/firstdemo/com.yourcompany.firstdemo_0.0.1-1_iphoneos-arm.deb’.
</pre>
3.make install 执行次操作之前确保iPhone安装了OpenSSH,并且在同一局域网.
<pre>
$ make package install
Making all for application firstdemo…
make[2]: Nothing to be done for `internal-application-compile’.
Making stage for application firstdemo…
 Copying resource directories into the application wrapper…
dpkg-deb: building package ‘com.yourcompany.firstdemo’ in ‘/Users/author/Desktop/firstdemo/com.yourcompany.firstdemo_0.0.1-1_iphoneos-arm.deb’.
...
root@ip’s password: 
...
</pre>
这个过程会提示你输入几次iphone或者ipad的密码。默认是:alpine.
##9.构建一个Tweak
本文介绍如果hook ios中任意类的方法的例子.

1.下载libsubstrate.dylib到/opt/theos/lib.

2.class-dump相关头文件到/opt/theos/include目录下

3.创建tweak项目
执行 $THEOS/bin/nic.pl,选择5
<pre>
author$ $THEOS/bin/nic.pl
NIC 1.0 - New Instance Creator
——————————
  [1.] iphone/application
  [2.] iphone/library
  [3.] iphone/preference_bundle
  [4.] iphone/tool
  [5.] iphone/tweak

Choose a Template (required): 5
Project Name (required): WelcomeWagon 
Package Name [com.yourcompany.welcomewagon]: 
Author/Maintainer Name [Brandon Trebitowski]: 
MobileSubstrate Bundle filter [com.apple.springboard]: 
Instantiating iphone/tweak in welcomewagon/…
Done.
</pre>
4.改写Tweak.xm文件
一旦你创建了项目，你会发现Theos生成了一个叫做Tweak.xm的文件，这是个特殊的文件，hook的相关代码就将写在这个文件。
<pre>
#import 
//引入相关的dump出的头文件

%hook SpringBoard
//hook的类名

-(void)applicationDidFinishLaunching:(id)application {
%orig;
//执行原方法

UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Welcome" 
message:@"Welcome to your iPhone Brandon!" 
delegate:nil 
cancelButtonTitle:@"Thanks" 
otherButtonTitles:nil]
[alert show];
[alert release];
}
//新增的hook代码

%end
//代码结束
</pre>
5.更改makefile文件,加入相关的类库

WelcomeWagon_FRAMEWORKS = UIKit

6.make, make package, make install

#参考资料
1.http://wufawei.com/2013/08/iOS-jailbroken-programming-1/