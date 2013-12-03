---
layout: post
title: "[git权威指南]读书笔记"
date: 2013-12-03 14:13
comments: true
categories: os

---
#第四章 git初始化
##1设置git别名可以使用更加简洁的命令
###1)希望注册的别名可以被所有的用户使用
<pre>
sudo git config --system alias.st status
</pre>

###2)希望注册的别名仅仅被本用户使用
<pre>
git config --global alias.st status
</pre>
###3)git在输出中开启颜色显示
<pre>
git config --global color.ui true
</pre>
###4)一些初始化的操作
<pre>
mkdir demo
cd demo
git init
</pre>
输出:
<pre>
Initialized empty Git repository in /Users/lee/sourceCode/gitTest/demo/.git/
</pre>
从上面的输出可以看出,创建了隐藏的名为.git的目录,这个目录就是Git版本库,也叫仓库.

为其添加一些文件:
<pre>
echo "Hello." > welcome.txt
</pre>
这样我们就可以进行一次提交
<pre>
git add welcome.txt
</pre>
git会强制要求进行提交说明,如果没有在命令行进行说明那么git就会默认打开一个编辑器.
<pre>
git commit -m "initialized"
</pre>
###5)git config命令的参数有什么区别

分别对应三个不同级别的配置文件:版本库级别配置文件,全局配置文件(用户主目录下)和系统级别配置文件(/ect目录下).优先级别也是按照此顺序,版本库>全局>系统级别.
配置文件采用的是INI文件格式,git config可以用于读取和更改INI配置文件中的内容,命令格式为:
<pre>
git config <section>.<key>	//读取
git config <section>.<key> <value>	//设置

git config core.bare
git config a.b something

//设置全局的姓名和邮件地址
git config --global user.name "monadogg"
git config --global user.email just4lee@qq.com
</pre>

###6)其他内容
如果没有设置全局的姓名和邮件,就无法直到提交者的信息.

###7>备份本章的工作成果
git clone demo demo-step-1


#第五章 git缓存区
可以使用git log查看提交日志,如果带上参数--stat可以查看每次提交文件变更统计.
<pre>
git log --stat
</pre>

###1)修改后不能直接提交么?
<pre>
echo "nice to meet you" >> welcome.txt
git diff
</pre>

可以通过git diff查看修改后的文件与版本库中的文件的差异(实际上和本地比较的是中间状态的文件而不是版本库中的文件).

使用git status可以查看文件状态.

###2暂存区的秘密
.git/index实际上就是一个包含文件索引的目录树,像是一个虚拟的工作区,其中记录了文件名和文件的状态信息,文件内容没有存储再其中,而是保存再git对象库.git/objects目录中,文件索引建立了文件和对象库中对象实体之间的对应.(以下内容如果有的地方想不明白,看后续的文章也许就会明白)

* 当对工作区进行修改(或新增)的文件执行git add命令的时候,暂存区的目录树会被更新,同时工作区的修改(或新增)的文件内容会被写入到对象库中的一个新的对象中,而该对象的id会被记录在暂存区的文件索引中.

* 当执行某次提交操作的时候(git commit),暂存区的目录树会写到版本库(对象库)中,master分支会做相应的更新,即master最新指向的目录树就是提交时原暂存区的目录树.

* 当执行git reset HEAD命令的时候,暂存区的目录树会被重写,会被master分支所指向的目录树所替换,但是工作区不受影响.

* 当执行git rm --cached <file>命令时,会直接从暂存区删除文件,工作区则不会做出改变.

* 当执行git checkout . 或者 git checkout -- <file>命令时,会用暂存区全部的文件或指定的文件文件替换工作区的文件,这个操作很危险,会清除工作区中为未添加到暂存区的改动.

* 当执行git checkou HEAD .或者git checkout HEAD <file>命令时,会用HEAD指向的master分支的全部文件或者部分文件替换暂存区和工作区的文件,这个命令也是很危险的,因为不但会清除工作区中未提交的改动,也会清楚暂存区中未提交的改动.

###3.git diff魔法
本章目前出现了工作区,暂存区和版本库(当前分支,HEAD)三个不同的目录树.

如何查看暂存区和HEAD(版本库当前的提交)中的目录树?
<pre>
git ls-tree -l HEAD

100644 blob 26a21daa71bbf9129789b81d18c04f8a75f0bb9f      24	welcome.txt
</pre>

第一个字段是文件的属性,第二个说明的是文件的类型,第三个说明的是文件在对象库中对应的id,第四个字段说明的文件的大小,第五个字段是文件名.

在浏览暂存区的目录树之前,需要清除工作区当前的改动,清除当前工作区没有加入版本库的文件和目录(非跟踪文件和目录),然后执行git checkout .命令,用暂存区内容刷新工作区.然后做出一些修改,并添加到暂存区,最后再对工作区做出修改.

<pre>
echo "bye...88" >> welcome.txt
lee-macbook-pro:demo lee$ mkdir -p a/b/c
lee-macbook-pro:demo lee$ echo "hello2" >a/b/c/hello.txt
lee-macbook-pro:demo lee$ git add .
lee-macbook-pro:demo lee$ echo "bye~~~" >> a/b/c/hello.txt
lee-macbook-pro:demo lee$ git status -s

AM a/b/c/hello.txt
M  welcome.txt
</pre>
上述命令运行完毕之后,工作区,暂存区,和版本库当前分支的最新版本(HEAD)各不相同,要显示暂存区的目录树可以使用git ls-files命令.

<pre>
$ git ls-files -s
100644 14be0d41c639d701e0fe23e835b5fe9524b4459d 0	a/b/c/hello.txt
100644 1a73102d46407057acaa040f1ebb6e175d366506 0	welcome.txt
</pre>
其中需要注意的是第三个字段不是文件的大小,而是暂存区编号.

如果想针对暂存区的目录树使用git ls-tree命令,需要先将暂存区的目录树写入Git对象库(git write-tree命令),然后针对该目录树执行git ls-tree命令.
<pre>
$ git write-tree
23fbfeddde830a81e25eb8f18c570eb6029fad74

$ git ls-tree -l 23fb
040000 tree f7a6bdc6b9b6555a6db7092610d67a26a76e86a5       -	a
100644 blob 1a73102d46407057acaa040f1ebb6e175d366506      54	welcome.txt
</pre>

* 命令git write-tree的输出就是写入Git对象库中的Tree ID,这个ID作为下一条命令输入.
* git ls-tree命令中,没有把40位的id写全,只要不冲突,可以随意简写.
* 可以看出第一条是个tree对象,第二条记录是一个文件.

通过使用不同的参数调用git diff命令就可以对工作区,暂存区和HEAD的内容进行两两比较.


**1.工作区和暂存区比较**
git diff

**2.暂存区和HEAD比较**
git diff --cached

**3.工作区和HEAD比较**
git diff HEAD


###3.其他的说明
不要使用git commit -a
该命令可以对本地所有变更的文件执行提交操作,包括本地修改的文件和删除的文件,但是不包括未被版本库跟踪的文件.

搁置问题,暂存状态
<pre>
$ git status
# On branch master
# Changes to be committed:
#   (use "git reset HEAD <file>..." to unstage)
#
#	new file:   a/b/c/hello.txt
#	modified:   welcome.txt
#
# Changes not staged for commit:
#   (use "git add <file>..." to update what will be committed)
#   (use "git checkout -- <file>..." to discard changes in working directory)
#
#	modified:   a/b/c/hello.txt
#

</pre>


git体贴得告诉用户,如果将暂存区的文件从暂存区撤离,以便让暂存区和HEAD一致,这样就不会发生提交.

还告诉用户,暂存区更新后在的工作区再一次修改有两个选择,一个是添加到暂存区,一个是取消工作区做出的改动.目前保存当前的工作进度所以使用:
<pre>
git stash

git status //所有的改动都不见了
</pre>



HEAD和master可以相互替换,一次提交包含了三个SHAI哈希值标识的对象id,分别是:本次提交的唯一标识,本地提交对应的目录树,本次提交的父提交.

git branch	可以显示当前的分支

#第六章

master的实现就是master分支在版本库的引用目录(.git/refs)中体现为一个引用文件.git/refs/heads/master,其内容就是分支最新提交的ID.


#第七章

引用refs/heads/master就像是一个游标,再有新的提交发生的时候,就指向提交,git提供了git reset命令可以将"游标"指向任何一个存在的提交id,如果使用了--hard参数,会破坏工作区未提交的改动.

HEAD^ 代表HEAD的父提交

<pre>
git reset --hard HEAD^
git reset --hard 9e8a761
</pre>
重置命令很危险,会彻底丢弃历史,而且不能恢复.


git reset 命令详解
<pre>
git reset [-q] [<commit>] [--] <paths>…
</pre>
在命令中包含路径<paths>为了避免路径和引用(或者提交的ID)同名而发生冲突,可以在<paths>前用两个连续的短线作为分割.这种用法不会重置引用,也不会改变工作区,而是制定提交状态(<commit>)下的文件(<paths>)替换暂存区的文件,例如命令:

git reset HEAD <path>相当于取消之前执行的git add <paths>命令的时候改变的暂存区.

<pre>
git reset [--soft | --mixed | --hard |--merge | -keep] [-q] [<commit>]
</pre>

会重置引用,根据不同的选项,可以对暂存区或者工作区进行重置

(1).替换引用的指向,引用指向新的提交ID

(2).替换暂存区,替换后,暂存区的内容和引用指向的目录树一致

(3).替换工作区,替换后,工作区的内容会变得和暂存区一致,也和HEAD所指向的目录书内容一致.

--hard,会执行操作(1)(2)(3).

--soft进行操作(1),只改变引用指向,不改变暂存区和工作区.

--mixed(默认参数),会执行操作(1)(2),会改变引用的指向及重置暂存区,但是不改变工作区.


实例:
<pre>
git reset
git reset HEAD 
仅用HEAD指向的目录树重置暂存区,工作区不受影响,相当于之前用git add命令更新到暂存区的内容撤出暂存区,引用,也没有改变.

git reset -- filename
git reset HEAD filename
仅将文件filename的改动撤离出暂存区,暂存区的其他文件不改变,相当于git add filename的反向操作.

git reset --soft HEAD^
工作区和暂存区不改变,但是引用向前回退一次,当对最新提交的提交说明,或提交的更改不满意时候,撤销最新的提交,以便重新提交.

git reset HEAD^
工作区不改变,但是暂存区会回退到上一次提交之前,引用也会回退一次.

git reset --mixed HEAD ^
同上

git reset --hard HEAD^
彻底撤销最近的提交,引用回归到前一次,而且工作区和暂存区都会回退到上一次提交的状态,自上一次以来的提交全部丢失.
</pre>

p127
