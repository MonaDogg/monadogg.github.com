---
layout: post
title: "常用unix命令"
date: 2013-12-03 11:11
comments: true
categories: os
---
###1.列出文件

ls 参数 目录名

###2.转换目录

cd

###3.建立新目录

mkdir 目录名 

mkdir /User/用户名/Desktop/backup

###4.拷贝文件

cp 参数 源文件 目标文件 

把驱动目录下的所有文件备份到桌面backup 

cp -R /System/Library/Extensions/* /User/用户名/Desktop/backup

###5.删除文件

rm 参数 文件

rm -rf /System/Library/Extensions.kextcache 

参数－rf 表示递归和强制，千万要小心使用，如果执行了 rm -rf / 你的系统就全没了

###6.移动文件

mv 文件 例：想把AppleHDA.Kext 移到桌面 

mv /System/Library/Extensions/AppleHDA.kext /User/用户名/Desktop

###7.文本编辑

nano 文件名
vim 文件名

###8.删除一个目录
rmdir dirname

###9.移动或重命名一个目录
mvdir dir1 dir2

###10.显示当前目录的路径名
pwd

###11.生成一个新文件
touch readMe.txt

###12.重复上次的命令

!!

###13.更改密码

passwd