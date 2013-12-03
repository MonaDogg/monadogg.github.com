---
layout: post
title: "又是一个github博客"
date: 2013-12-03 11:10
comments: true
categories: os
---

在github上拥有一个博客地址,本人觉得是一件挺酷的事情,于是乎翻遍了网上大部分的教程,在花费了一天半的时间后,终于搭建完成了,其中遇到了很多的坑,幸亏在时间的软磨硬泡下,一一被我解决了(主要是对ruby环境的不熟悉造成的).
#1.搭建ruby环境
搭建rvm环境和相关的设置

rvm是一个命令行工具，可以提供一个便捷的多版本ruby环境的管理和切换,下面的代码是用来安装rvm的.
<pre>
bash -s stable < <(curl -s https://raw.github.com/wayneeseguin/rvm/master/binscripts/rvm-installer)
</pre>
然后需要设置环境变量:
<pre>
echo '[[ -s "$HOME/.rvm/scripts/rvm" ]] && . "$HOME/.rvm/scripts/rvm" # Load RVM function' >> ~/.bash_profile
source ~/.bash_profile

# If using Zsh do this instead
echo '[[ -s $HOME/.rvm/scripts/rvm ]] && source $HOME/.rvm/scripts/rvm' >> ~/.zshrc
source ~/.zshrc
</pre>
最终需要安装ruby环境(不知道为何安装1.9.2会报奇怪的错,换成1.9.3就好了):
<pre>
rvm install 1.9.3 && rvm use 1.9.3
rvm rubygems latest
</pre>
#2.安装Octopress
克隆代码到当前目录下,并且进入octopress目录:
<pre>
git clone git://github.com/imathis/octopress.git octopress
cd octopress
</pre>
安装相关的依赖库:
<pre>
gem install bundler
bundle install
</pre>
最后安装Octopress
<pre>
rake install
</pre>
#3.简单设置你的博客
1)修改文件_config.yml,里面基本上是一些博客名称,作者姓名等等之类的设置信息.

2)修改source文件夹下一些html中twitter,google等政府屏蔽网站的相关资源,会造成页面加载缓慢,具体判断建议使用chrome,查看每项资源的加载时间,将无法获取的资源保存到本地,或者换为墙内的加载地址.

#4.设置github账号

在github上创建一个名为 账号名.github.com 的代码仓库。

#5.写博客

写博客的主要命令为：
<pre>
rake new_post[‘article name’] 生成博文框架，然后修改生成的文件即可
rake generate 生成静态文件
rake preview 在http://localhost:4000 访问博文内容
rake deploy 将生成的博文推送到你的github上
同时我也建议把所有的源码也推送到github进行保存,代码一般为:
git add .
git commit -m "add source files"
git push origin source
</pre>
#6.关于markdown
Markdown 是一种轻量级标记语言，创始人为约翰·格鲁伯（John Gruber）和亚伦·斯沃茨（Aaron Swartz）。它允许人们“使用易读易写的纯文本格式编写文档，然后转换成有效的XHTML(或者HTML)文档”

我们在github上面存放的博客其实是通过ruby下的一个框架,将我们使用markdown语法编写的博文 转换为html网页.在这里赞赏下网友们的智慧.

而markdown的教程网上有很多,你可以先点击[这里](http://zh.wikipedia.org/wiki/Markdown)进行了解.

#7.美化我们的博客
如果你觉得我们安装好的博客挺丑的,而且评论功能也很难用,还有那个定制栏能不能放点别的什么,那我们就需要动手改造了,在这里我仅仅介绍下安装主题的方法.

我使用的主题是[whitelake](https://github.com/gehaxelt/CSS-WhiteLake):
<pre>
cd octopress
git clone https://github.com/gehaxelt/CSS-WhiteLake.git .themes/whitelake
rake install['whitelake']
rake generate
rake preview
</pre>
别的模块定制方法也许你应该看看[这个](http://biaobiaoqi.me/blog/2013/07/10/decorate-octopress/).
#参考文献:
1.首先感谢唐巧的博客,本文的大部分内容都是来自他的一篇[博文](http://blog.devtang.com/blog/2012/02/10/setup-blog-based-on-github/),作为iOS工程师我也建议你关注他们的微博,并且仔细研读他的每篇博文,真是收获颇丰.

2.http://ishalou.com/blog/2012/10/15/how-to-use-octopress/

3.http://hahaya.github.io/2013/06/26/build-blog-on-github.html

4.http://www.cnblogs.com/rubylouvre/archive/2012/06/10/2543706.html

5.http://easypi.github.io/blog/2013/01/05/using-octopress-to-setup-blog-on-github/

以下是美化博客:

6.http://yanping.me/cn/blog/2012/01/07/theming-and-customization/

7.http://biaobiaoqi.me/blog/2013/07/10/decorate-octopress/

8.http://xuhehuan.com/886.html

9.http://lucifr.com/2012/02/05/slash-theme-for-octopress/