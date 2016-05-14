---
title: Windows下 TortoiseGit+Coding 的使用
date: 2015-07-15
categories: 技术备忘
tags: git
---
git可是个好东西，版本控制嘛，不满意回滚呗，有异议开分支呗。

<!--more-->
### 为什么要用Git
简单来说，Git就是一个版本控制系统，它可以保存我们对代码的每次修改，也可以创建多个分支，必要时可以返回修改。

那么Git比我们熟知的svn又有哪些优点呢？经验所限，我只能大概说出几点。

`1.Git会在自己电脑本地创建一个本地仓库`
为什么说这一点好呢？和svn不同的是，git因为本机带有一个本地仓库，所有修改操作都要先提交到本机，然后再推送出去。
这也意味着git可以缓存多个版本，而svn只能缓存一个，所以git相当于svn的客户端+svn的服务器端。
咋一看没什么实际的用处，假设你现在现在不能联网，与团队的其他人数据无法交互，同时这段时间你又对代码做了很大的修改，这也意味着git可以保存
每一模块修改的版本，如果需要恢复会很方便，如果是svn，只能返回到你大量修改前，相当于你做了大量的无用功。

`2.Git支持的代码托管平台比较多`
好像开源中国的Git@OSC支持svn，这点不确定，没用过，这也从侧面反映了svn的托管平台确实少。
git的就多了,基本上有名的托管平台都是支持的。
像大名鼎鼎的`gitHub`,支持无限的公有项目和私有项目`gitlab`,还有国内速度极快的`coding`等等，这个很多，有兴趣可以自己找下。

据说git可是Linus用C耗费两周写出来的，各种狂霸酷炫拽有木有，
具体教程可以参考[廖雪峰的教程](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)，写得很不错。

### 下载及安装所需工具

#### 下载[git for windows](http://git-scm.com/download/)
进入git的官网,选择windows版本下载，

![git](/img/pics/2015-07-15/gitdown.png)

![git](/img/pics/2015-07-15/gitDown2.png)


#### 下载[TortoiseGit](http://git-scm.com/download/)
习惯把Tortoise叫小乌龟，Tortoise也有svn版的，是Windows下的可视化界面。
选择最新的当前版本，现在最新的是1.8.14.0，然后选择 32bit/64bit下载。

![tortoiseGit](/img/pics/2015-07-15/TortoiseGit.png)

![tortoiseGit](/img/pics/2015-07-15/TortoiseGit2.png)

#### 安装git
把第一、三、四个一级复选框都勾上，因为我们用小乌龟做图形界面，所以不需要安装git gui
![installGit](/img/pics/2015-07-15/installGit.png)

 下面选择use git from the windows command prompt
 选择这个会将git配置到path里面
![installGit](/img/pics/2015-07-15/installGit2.png)

这个是选择回车换行的格式，默认第一个即可，检出时转换为Windows风格,提交时转换为Linux风格
![installGit](/img/pics/2015-07-15/installGit3.png)

接下来应该没什么问题了，结束。

#### 安装TortoiseGit
小乌龟安装更简单了，默认，直接下一步，结束。

![tortoiseGit](/img/pics/2015-07-15/installTortoise.png)

![tortoiseGit](/img/pics/2015-07-15/installTortoise2.png)

### 注册coding.net账号
由于我们选择在coding创建远程仓库，所以注册一个[coding.net](https://coding.net)的账号是必须滴。

![coding](/img/pics/2015-07-15/coding.png)


需要一个邮箱和独一存在的用户名，或者说是个性后缀，都可以。
由于我已经注册过，下面的邮箱和用户名显示已存在，这个没什么可说的，自己注册个。

![coding](/img/pics/2015-07-15/coding2.png)

### 创建本地仓库并配置#

#### 创建本地仓库文件夹
在你对应的工作空间创建一个空的文件夹`test_demo`
右键`test_demo`文件夹，选择Git Create repository here。
第一次会弹出git init的对话框，不要勾选make it bare，ok。
这样就选择这个文件夹作为你的本地仓库，这个文件夹也是你的项目。每次修改代码会先提交到这个地方，然后再从这里推送到远程仓库。

#### 配置git日志标识
右键`test_demo`文件夹，在`TortoiseGit`一栏选择setting，在弹出的窗口点击左边的git节点，
填入name和email，这个只是作为你的这个项目的本地机器的日志标识，关系并不大，可以随便填。

![setting](/img/pics/2015-07-15/setGit.png)


#### 配置访问方式
假如在远程仓库上，你创建了一个项目，
一般有三种访问方式
1) HTTPS：读写仓库加密通道，有单次上传限制。
2) SSH：读写仓库加密通道，无单次上传限制，需先在设置公钥，完成配对验证。
3) Git：只读，并且只对公开项目有效。

>`如果要使用ssh方式访问`
>ssh方式是一种加密的访问方式，使用这种方式我们需要在本机机器生成一串公钥，然后在托管平台上进行绑定授权。
我们安装小乌龟会附带安装一些组件，找到`Puttygen`这个程序打开，点击Generate按钮，接着你会发现随着你鼠标在`Puttygen`窗口滑动，进度条会滚动并最终生成一串公钥
![setting](/img/pics/2015-07-15/setGit2.png)
`ssh-rsa AAAAB3NzaC1yc2EAAAABJQAAAIB+gadTNAA3iID1l5rg0qEhn5YySxANkYS6pQykbvdq8p
V70dIFrsN2QCxoVz7W0NZydgFSAwGc4TYWKCSJUKVqgg+2Sf2mFc5LwaHwOX+NilA6SwH2NEOd1lUmr
MgV7HJ8f4OhL4Ivwoqw/ZRPfBURfuHpVSXol5T4XuqMS00G0w== rsa-key-20150715`
上面这一串就是了，copy下。
接着点击 `save private key` 把私钥保存在本地，注意！你的公钥对应的私钥必须要妥善保存，如果丢失跟密码被盗一样严重。记住路径和文件名，等下要用到。

打开`coding.net`,当然肯定要登陆了，然后创建一个新项目，中文界面滴，看不懂找你小学语文老师。

![init](/img/pics/2015-07-15/import.png)


在这里我选了启用readme.md来初始化，这个没什么关系，一个介绍页面而已，不过写这个东东还是挺有趣的，一种新的语法应用。

![init](/img/pics/2015-07-15/import2.png)


可以看到，默认创建了一个master分支，然后我——Tornado作为项目创建者也默认添加进了项目成员里面，`如果是团队协作，需要先把人邀请到这个项目，他们才能访问`。


>`如果是ssh方式访问`
>在个人账户信息配置ssh公钥，把刚才copy的粘贴上去，名称随便填，不过最好稍微标记下是哪台电脑。
![init](/img/pics/2015-07-15/t.png)
![init](/img/pics/2015-07-15/t2.png)

打开`coding.net`,点击`代码`选项卡，可以看到这个。

![init](/img/pics/2015-07-15/v.png)

左下角那个选择ssh或者https方式访问，copy下那串链接。

打开项目的`setting`，选择git节点下的remote。

![init](/img/pics/2015-07-15/v2.png)

在url那栏把刚才copy的访问链接粘贴下去，`putty key`，ssh访问需要导入私钥路径，https则不需要，`remote`标记这个这条访问链，随便填。填好后点击`Add new / save`。
ok后接着下一步，出现下面这个。

成功了，就会提示`Success`，不成功的话就检查下信息是否有误或者前面哪个操作出现问题，再试一次。

### 初始化项目及推送项目
这样配置的东西都搞定了，不过由于我们本地仓库是空的，需要从远程仓库`pull`下来初始化一下。
右键`test_demo`文件夹，在`TortoiseGit`里面选择`pull`，`Success`后可以看到项目里面多了个`readme.md`文件。

接下来在项目里面随便建立一个`test.php`文件，刚建立可以看到图标是一个问号，表明还没加入到本地仓库。右键这个文件，在`TortoiseGit`里面选择`add`，会弹出一个对话框，我们直接提交到本地仓库，点击`commit`按钮

![init](/img/pics/2015-07-15/c.png)

会弹出一个确定操作的窗口，可以看到你对什么文件进行提交，我们勾选`test.php`文件，在`message`修改操作信息，这个是必填的。

>`如果是https方式访问`
![init](/img/pics/2015-07-15/v3.png)
>推送的时候会弹出这个，提示要输入用户名和密码，每次推送都需要输一次，像我这么懒的人就受不了了。
我们可以点击`setting`下git节点下的`Edit local.git/config`按钮，在弹出的
Notepad文本的最后输入。
`[credential]
helper = store`
这样就只要输入一次就会自动保存，不过需要注意的是这种方式会在本地以明文形式保存信息，具体信息可以自行拓展。

当看到`test.php`文件图标变成绿色对勾时，就证明提交成功了。


接着往coding.net推送，我们可以选择整个项目，也可以选择单个文件推送，
右键`test_demo`，在`TortoiseGit`里面选择`push`，没什么意外都是直接ok。
`success`后打开`coding.net`看下动态，可以看到最新的推送信息。至此完成。

![init](/img/pics/2015-07-15/c2.png)

### 想说的话
首先非常感谢各位耐心的看到了最后，第一次写博文，可能会存在很多不足，希望各位路过的大牛不吝赐教，如果对于文章有什么疑惑或者建议的，也希望你们在下面的多玩评论里面留下你们的脚印，谢谢。

### 参考
[ethan_xue](http://blog.csdn.net/ethan_xue/article/details/7749639)
[Bonker](http://www.cnblogs.com/Bonker/p/3441781.html)
[铁锚](http://blog.csdn.net/renfufei/article/details/41647875)
