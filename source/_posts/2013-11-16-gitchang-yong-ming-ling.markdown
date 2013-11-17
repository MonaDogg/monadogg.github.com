---
layout: post
title: "[持续更新]git常用命令"
date: 2013-11-16 22:35
comments: true
categories: Git
---
如果读者想成git高手,本文也许并不适合你,我在这里推荐一本书为<Git权威指南>,这本书会介绍git的方方面面,如果读者是一位并不清楚git基础知识的读者,那么建议读者先从文章末尾的推荐阅读的资料开始学习git.本文主要记载日常使用git中常用的命令,并尽量讲清楚使用该命令的使用场景.
	


#1.本地操作
##1.1初始化
###1.1.1全局变量
git config --global user.name "xxx"

git config --global user.email "xxx@qq.com"

git config --global color.ui "always"
###1.1.2初始化新版本库
git init 只会在根目录下建立一个名为.git文件夹
###1.1.3设置忽略的文件
**1.设置每个人都需要忽略的文件**

1)在根目录新建一个名为.gitignore的文本文件

2)在命令行执行echo \*.jpg>.gitignore,注意>左右不要有空格

3)将.gitignore文件加入版本库并提交

**2.设置只有自己忽略的文件**

1)修改.git/info/exclude文件

2)可以使用正则表达式,例如\*.[oa]等价于\*.o和\*.a



###1.1.4添加文件到版本库
**1.添加单个文件**

git add somefile.tet

**2.添加所有txt文件**

git add *.txt

**3.添加所有文件**

git add . 包含子目录但是不包含空目录

###1.1.5提交
**1.提交**

git commit -m "add all files"


##1.2日常操作



#2.远程操作
##2.1初始化
##2.2日常操作
##2.3github