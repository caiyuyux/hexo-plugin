---
title: 如何使用JavaScript调用Disqus API
date: 2016-10-24
categories: [技术备忘]
tags: [Disqus, JavaScript]
---

第三方评论系统存在的意义？绝对不是可以让开发者偷懒而存在的，虽然确实可以偷下懒，哈哈。

<!--more-->

### Disqus 是什么？

Disqus就是一个评论系统，广泛应用于新闻媒体、博客等社交平台，提供评论的接口，聊天呀留言呀交互呀，一个意思。这种评论系统国外大多是用Disqus，在国内Disqus好像被墙了，需要科学上网，不过类似的有多说、友言等，都差不多，只是丑了点，支持的社交账号也不太一样，看自己需要了。在我看来，第三方评论系统的一个好处在于可以搭载在静态网站上来实现“动态”，这也是其广泛运用于个人博客的原因。


### 为什么使用API？

可能有人有点疑惑，使用了Disqus，官网自然有后台让你管理自己的账号，为什么需要自己调用API搭建管理系统？呃呃，那自然是为了一体化服务呀，你想想，你一个网站或者系统尽善尽美，各种接口都提供了，但是管理下小小的评论还要登下Disqus去管理，那不是打脸吗？

### 准备事项

1. Disqus账号
2. 创建一个API应用程序

账号自然不必说，而且Disqus应该也要安装上了且可以使用了，这个跳过。  

其次是需要创建一个应用程序，点[这里](https://disqus.com/api/applications/)先注册一个应用程序，然后你会得到几个授权凭据，这里我们只需要`API Key`和`Access Token`就可以了。

### JS调用API

我们这里是前端实现的，也可以后台调用，点[这里](https://disqus.com/api/docs/)，有官网提供的API文档介绍，挺详细的，主要是给你个URL链接让你获取Json或jsonp对象来传输数据。

```javascript
$(document).ready(function() {

    $commentDiv = $("#comments");

    var forum = <your short name>;
    var APIKEY = <your API key>;
    var ACCESS_TOKEN = <your access token>;

    $.ajax({
        type : "get",
        url : "https://disqus.com/api/3.0/forums/listPosts.json",
        data : "related=thread&forum="+forum+"&api_key="+APIKEY+"&access_token="+ACCESS_TOKEN+"&include=approved&include=spam&include=deleted",
        async : false,
        success : function(res, code){
            if(res.code === 0) {
                var result = "";
                for(var i=0, len=res.response.length; i<len; i++) {
                    var post = res.response[i];
                    console.dir(post);//这个用来调试很方便
                    var html = "<div class='comment'>";
                    html += "<img src='" + post.author.avatar.small.permalink + "'>";
                    html += "<a href='"+ post.author.profileUrl + "'>" + post.author.name + "</a>";
                    html += "<p>"+post.raw_message+"</p>";
                    html += "<p>Posted at " + post.createdAt + " on <a href='"+ post.thread.link + "'>" + post.thread.title + "</a></p>";
                    html += "</div>";
                    
                    result+=html;
                }
                $commentDiv.html(result);
            }
        }
    });

});
```

这个是遍历评论的一个实现片段，需要注意一点的是，`ACCESS_TOKEN`这个字段部添加也可以，但是添加后获取的对象会多出评论人的邮箱和IP等私密信息，所以需要进一步验证。

```javascript
    jQuery.post('https://disqus.com/api/3.0/posts/update.json',{
        post:"2957858030",//这个看你具体获取到什么内容
        access_token:ACCESS_TOKEN,
        message:"This comment has been overwritten",
        api_key:APIKEY})
```

这个是对评论进行更新，此外，Disqus提供一个控制台调试很有用，点[这里](https://disqus.com/api/console/#!/)

### 参考
[Raymond Camden](https://www.raymondcamden.com/2014/03/21/Example-of-a-JavaScript-Disqus-Recent-Comment-Widget/)