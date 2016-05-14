---
title: Windows下 Emacs+Markdown 的使用
date: 2015-08-29
categories: 技术备忘
tags: [emacs, Markdown]
---
emacs竟然不能预览markdown，不可能，还有什么是没万能的emacs做不到的？
<!--more-->
### 解决什么问题？
简单的来说，本文解决的是你在emacs下使用 `C+c C+c p` 指令预览markdown文件发生的错误，类似如下。

>‘markdown’不是内部或外部指令，也不是可运行的程序或批处理文件。

如果是这个问题的话，恭喜你，继续往下看。

一般来说，我们使用markdown会输出成html格式才可以使用。除非网站可以解析md文件，比如说github，那就可以省了这个步骤。
所以我们要预览就肯定要导出一个html文件，而md文件转成html文件是要添加外部依赖的，这就是问题的根源。

这里我用了两种方法来解决。

+ 使用python的Markdown包
+ 使用pandoc工具

### 使用python的markdown包

####搭建python环境
下载python ([https://www.python.org](https://www.python.org))
然后安装，这个简单，就不用我说了，无脑操作。

####下载Markdown包并安装
打开`cmd`，输入如下，会自行下载安装，python的环境变量如果没有设置要先设置好。

>pip install markdown

![down](/img/pics/2015-08-29/downMarkdown.png)

安装好它会提示你安装的位置，进入该目录的包文件，你会看到有个`setup.py`文件，这个是python脚本的安装文件，
打开`cmd`，cd到该位置，输入指令

>python setup.py install

![install](/img/pics/2015-08-29/installMarkdown.png)

又是一大堆信息出来，个人觉得python这点很好，可以让我们很清楚的知道产生了那些文件。
我们可以看到在`Scripts`文件夹下产生了一个名为`markdown_py`的批处理文件。

####修改markdown-mode.el
我们生成的批处理文件名是`markdown_py`，但是`markdown-mode.el`里面默认的指令名是`markdown`,所以需要修改下。

打开`markdown-mode.el`文件，在800多行左右可以看到如下

![el](/img/pics/2015-08-29/el.png)

将`markdown`改成`markdown_py`

![el2](/img/pics/2015-08-29/el2.png)

再打开emacs，切换到`markdown-mode`，随便输点东西，`C-c C-c p`一下，运行如下。

![t](/img/pics/2015-08-29/t.png)

成功运行，没有报出指令错误，但是我们可以发现浏览器中文乱码。

这个简单，因为md文件最终也是要转成html的，我们可以在在每个md文件开头加上一句

>`<meta http-equiv="content-type" content="text/html; charset=UTF-8">`

![t2](/img/pics/2015-08-29/t2.png)

不过确实，在md文件加上这一句确实不太美观，所以我推荐第二种方法，更好更强大。

###使用pandoc工具

####下载pandoc并安装
下载pandoc ([http://www.pandoc.org](http://www.pandoc.org))，选择windows版本，然后安装。

pandoc可以解析markdown文件，但是它并不仅仅支持markdown的标记语法，还支持语法高亮和LaTex数学公式，可以把它当作markdown的拓展。

以下是官网介绍，可以简单看一下。

![pandoc](/img/pics/2015-08-29/pandoc.png)

####修改markdown-mode.el
安装完pandoc后，环境变量会多出一个`pandoc`的指令，

打开`markdown-mode.el`，还是刚才那个位置

![](/img/pics/2015-08-29/el.png)

`pandoc`的命令选项如下

||选项||描述|||
||`-f html`||转换成html文件||
||`-t markdown`||解析markdown||
||`--ascii`||解决中文乱码||
||`--highlight-style pygments`||支持语法高亮||
||`--mathjax`||支持 LaTex数学公式||

一股脑都加进去吧，将`markdown`修改为如下

>`pandoc -f markdown -t html --ascii --highlight-style pygments --mathjax`

![well](/img/pics/2015-08-29/well.png)

水平所限，欢迎指点交流，谢谢。

### 参考
[辛未羊的博客](http://panqiincs.github.io/)
