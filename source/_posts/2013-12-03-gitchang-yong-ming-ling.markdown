---
layout: post
title: "git常用命令"
date: 2013-12-03 11:12
comments: true
categories: OS
---
如果读者想成git高手,本文也许并不适合你,我在这里推荐一本书为<Git权威指南>,这本书会介绍git的方方面面,如果读者是一位并不清楚git基础知识的读者,那么建议读者先从[这里](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)开始学习git.本文主要记载日常使用git中常用的命令,并尽量讲清楚使用该命令的使用场景.
#1.本地操作
##1.1初始化
###1.1.1全局变量
<pre>
git config --global user.name "xxx"

git config --global user.email "xxx@qq.com"

git config --global color.ui "always"
</pre>

###1.1.2初始化新版本库
<pre>
git init 只会在根目录下建立一个名为.git文件夹
</pre>

###1.1.3设置忽略的文件
1.设置每个人都需要忽略的文件

1)在根目录新建一个名为.gitignore的文本文件

2)在命令行执行echo *.jpg>.gitignore,注意>左右不要有空格

3)将.gitignore文件加入版本库并提交

2.设置只有自己忽略的文件

1)修改.git/info/exclude文件

2)可以使用正则表达式,例如*.[oa]等价于*.o和*.a

###1.1.4添加文件到版本库

1.添加单个文件
<pre>
git add somefile.tet
</pre>
2.添加所有txt文件
<pre>
git add *.txt
</pre>
3.添加所有文件
<pre>
git add . 包含子目录但是不包含空目录
</pre>
###1.1.5提交
1.提交

git commit -m "add all files"
##1.2日常操作
###1.2.1提交
1.提交所有修改
<pre>
git commit -m "some message" -a
</pre>
2.提交单个文件
<pre>
git commit -m "some message" readMe.txt
</pre>
3.增补提交
<pre>
git commit -C head -a -amend 
</pre>
不会产生新的提交历史记录
###1.2.2撤销修改
1.撤销尚未提交的修改

1)撤销少量文件
<pre>
git checkout head readMe.txt toDo.txt
</pre>
2)撤销所有txt文件
<pre>
git checkout head *.txt
</pre>
3)撤销所有文件
<pre>
git checkout head
</pre>
2.撤销提交后的修改

1)撤销某次提交,但是这次操作也会作为一次提交保存
<pre>
git revert -no-commit head
</pre>
2)复位,将当前head内容重置,会留下痕迹
<pre>
git reset head/git reset head <filename>
</pre>
3)复位到head之前的版本,不会在版本库留下痕迹
<pre>
git reset --hard head^
</pre>
4)永久删除最后3个commit(即HEAD, HEAD^和HEAD~2)
<pre>
git reset --hard head~3
</pre>
###1.2.3分支
1)列出本地分支
<pre>
git branch
</pre>
2)列出所有分支
<pre>
git branch -a
</pre>
3)基于当前分支末梢创建新分支
<pre>
git branch <branchname>
</pre>
4)检出分支
<pre>
git checkout <branchname>
</pre>
5)基于当前分支的末梢创建新分支并检出分支
<pre>
git checkout -b <branchname>
</pre>
6)基于某次提交/分支/标签 创建新分支
<pre>
git branch emputy bfe57de0      //用来查看某个历史断面很方便

git branch emputy2 emputy       //基于分支创建分支
</pre>
7)合并分支

1.普通合并:把两条分支以上的历史轨迹合并，交汇到一起
合并并提交,如果发生冲突就不会自动提交,如果冲突很多,不想立即解决他们,
可以直接使用git checkout head撤销所有尚未提交的修改.
<pre>
git merage <branchname>
</pre>
合并但并不提交
<pre>
git merage --no--commit
</pre>
2.压合合并:将一条分支上的若干个提交条目合成一个提交条目，提交到另一个分支末梢。

压合合并并提交
<pre>
git merge --squash <branchname>
</pre>
压合合并并不提交
<pre>
git merge --squash --no-commit <branchname>
</pre>
3.拣选合并:拣选另一条分支上的某个提交条目的改动带到当前分支上。
挑选某次提交合并但不提交
<pre>
git cherry-pick --no-commit 5b62b6
</pre>
8)重命名分支
<pre>
git branch -m <branchname><newname> 不会覆盖已存在同名分支

git branch -M <branchname><newname> 会覆盖已存在的同名分支
</pre>
9)删除分支
<pre>
git branch -d new2  如果分支没有被合并会删除失败

git branch -D new2  如果分支没有被合并也会被删除
</pre>
###1.2.4解决冲突
1)冲突很少时,直接编辑冲突文件提交即可
###1.2.5标签
1)创建标签

1.为当前分支最近一次提交创建标签
<pre>
git tag 1.0 //标签无法重新命名
</pre>
2.为其他分支最近一次提交创建标签
<pre>
git tag tagName branchName
</pre>
3.为某次历史提交创建标签
git tag 1.1 4e6861d5
2)显示标签列表
<pre>
git tag
</pre>
3)检出标签
<pre>
git checkout 1.0    //查看标签断面很重要,但是不能提交
</pre>
4)由标签创建分支
<pre>
git branch b1.1 1.1
git checkout -b b1.1 1.1
</pre>
5)删除标签
<pre>
git tag -d 1.1
</pre>
###1.2.6查看状态
1)当前状态
<pre>
git status
</pre>
2)历史记录
<pre>
git log

gitk    //查看当前分支的历史记录

gitk <branchname>    //查看某分支历史记录

gitk --all  //查看所有分支

git branch -v   //每个分之最后的提交
</pre>

###1.2.7其他

1)git导出项目,更多用法请参照git help archive
<pre>
git archive [options] <tree-ish> [<path>…]

git archive --format zip -o filename.zip HEAD

git archive --format zip -o filename.zip source
</pre>

一其中tree-ish可以是:
<pre>
HEAD
Tags
Branch names
Branch names with remotes, like origin/somebranch
</pre>
#2.远程操作
##2.1初始化
1)克隆版本库
<pre>
git clone <url>
</pre>
2)别名
<pre>
git remote add <别名> <远程版本库的URL>   //添加远程版本库的别名

git remote rm <别名>       //删除远程库的别名和相关分支
</pre>
##2.2日常操作
1)分支
<pre>
git branch -r   //列出远程分支

git remote prune origin     //删除远程库中已经不存在的分支
</pre>
2)从远程获取

1.获取远程版本库但是并不合并
<pre>
git fetch <远程版本库>  //获取但不合并

git fetch origin    //origin是远程库的别名

git fetch d:\git\source     //本地版本库
</pre>
2.获取远程版本库并且和当前分支合并
<pre>
git pull origin 

git pull d:\git\source master
</pre>
3)推入远程库
<pre>
git push origin master  //推入远程库
</pre>

#参考文献:
1.本文内容来自:weibo.com/crespoxiao的微博配图