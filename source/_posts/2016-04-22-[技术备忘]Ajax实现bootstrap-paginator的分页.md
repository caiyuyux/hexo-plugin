---
title: "[技术备忘]Ajax实现bootstrap-paginator的分页"
date: 2016-04-22 18:41:20
categories: 技术备忘
tags: [bootstrap, ajax, paginator]
---
网站开发实现分页是不可避免的，找到个bootstrap-paginator，最主要是挺漂亮的有木有？
<!--more-->
### 什么是分页？

这个就不废话了，分页分客户端分页和服务端分页，对于数据量比较大的情况，无疑是服务端分页优选，更快更便捷，本文讲的就是服务端分页，当然也可以做成客户端的效果，看你怎么实现。

***

### Paginator 是什么？

Bootstrap Paginator 一款基于Bootstrap的js分页插件，至于什么是bootstrap是什么就不用说了，前端框架，用起来很方便，漂亮简洁，不用费心去找模板了。

***


### 如何使用Paginator？

**声明**：我这里的代码是参照我的项目的，我直接复制过来，有些不是必要的，各位自行斟酌。

#### 导入库文件

```html
<link type="text/css" rel="stylesheet" href="bootstrap.css">
<script type="text/javascript" src="jquery-2.1.4.js"></script>
<script type="text/javascript" src="bootstrap-paginator.js"></script>
```

#### 前端界面的html代码

```html
<div class="panel-body">
    <ul id="post" class="list-group" style="height: 430px;">
        <div style="width: 500px; text-align: center">
            <ul id="pageLimit" style="position: absolute; bottom: 0px; margin-left: auto; margin-right: auto"></ul>
        </div>
    </ul>
</div>
```

这个是个div，我想在这里放置一个列表，这里要说明下，`id="post"`这个`ul`放的是列表，`id="pageLimit"`这个`ul`放的是分页显示的按钮，不要搞混了。

#### Ajax和jquery代码  

```js
 function Bootstrap_paging(page){
        var listNum = 2;//每页多少条数据
        $.ajax({
            type: "GET",
            url:"/PostGetData",
            dataType:"json",
            data: "pageId="+page+"&listNum="+listNum,
            success:function(msg){
                var element = $('#post');
                var pages = Math.ceil(msg.num/listNum)
                $('.postList').remove();
                $.each(msg.post,function(index,item){
                    element.append('<li class="list-group-item postList" onclick="getContent(this)">'+item.title+'<span style="float: right;">'+item.time);
                });
                var options = {
                    currentPage: page,
                    totalPages: pages,
                    useBootstrapTooltip: true,
                    bootstrapMajorVersion: 3,
                    tooltipTitles: function (type, page, current) {
                        switch (type) {
                            case "first":
                                return "Go To First Page <i class='icon-fast-backward icon-white'></i>";
                            case "prev":
                                return "Go To Previous Page <i class='icon-backward icon-white'></i>";
                            case "next":
                                return "Go To Next Page <i class='icon-forward icon-white'></i>";
                            case "last":
                                return "Go To Last Page <i class='icon-fast-forward icon-white'></i>";
                            case "page":
                                return "Go to page " + page + " <i class='icon-file icon-white'></i>";
                        }
                    },
                    bootstrapTooltipOptions: {
                        html: true,
                        placement: 'bottom'
                    }
                }
                $('#pageLimit').bootstrapPaginator(options);
            }
        });
    }
    $(function () {
        Bootstrap_paging(1);
    })
```

这里其实就是一个函数，传入的参数是需要跳转的页面，在第一次访问页面调用它，传入参数1，这样就显示了第一页的数据。  
`url`是后台的请求数据的页面，因为我们实现的是服务端分页，所以是一页一页的获取数据，需要传给后台的有俩个参数，  
一个是`pageId`，即当前的页面，一个是`listNum`，是每页最多显示多少条数据。


#### 修改bootstrap-paginator.js文件的代码

这个我觉得不是太好，因为bootstrap-paginator.js文件是一个库，一般我们尽量不要去改的，如果有需要修改的会提供一个接口给我们，但是我找了一下没有找到，所以只能直接在这里改了，  
如果有大牛路过知道怎么回事的希望不吝赐教。在该文件223行左右的地方加入刚才上面我们定义的函数。  

```js
    onPageClicked: function (event, originalEvent, type, page) {

        //show the corresponding page and retrieve the newly built item related to the page clicked before for the event return

        var currentTarget = $(event.currentTarget);

        switch (type) {
        case "first":
            currentTarget.bootstrapPaginator("showFirst");
            Bootstrap_paging(page);//add
            break;
        case "prev":
            currentTarget.bootstrapPaginator("showPrevious");
            Bootstrap_paging(page);//add
            break;
        case "next":
            currentTarget.bootstrapPaginator("showNext");
            Bootstrap_paging(page);//add
            break;
        case "last":
            currentTarget.bootstrapPaginator("showLast");
            Bootstrap_paging(page);//add
            break;
        case "page":
            currentTarget.bootstrapPaginator("show", page);
            Bootstrap_paging(page);//add
            break;
        }

    },
```

#### 后台服务器端的clojure代码

嗯，因为我这个后台是clojure实现的，所以这部分代码是clojure的，你们可以根据自己的语言写，功能挺简单。

```clojure
(defn PostGetData
  [request]
  (let [pageId  (parse-int (-> request :params :pageId))
        listNum (parse-int (-> request :params :listNum))]
    (def data (db/all_for_post))
    (def Newdata (hash-map :post (into [] (nth (partition-all listNum data) (- pageId 1))),
                           :num (count data)))
    (generate-string Newdata)))
```

如上，从数据库查询所需的全部数据，根据我们ajax异步请求传的`listNum`参数来确定切割几份，再根据`pageId`确定返回哪部分，自然，要转成json形式。
最后返回的数据大概是这样的。  

```json
{
    "num": 3,
    "post": [
        {
            "title": "title",
            "time": "Wed Apr 20 2016"
        },
        {
            "title": "aa",
            "time": "Wed Apr 20 2016"
        }
    ]
}
```

### 效果

![](/img/pics/2016-04-22/qq.png)

### 参考
[alleybag的教程](http://my.oschina.net/xiaoxiangdaizi/blog/489047?fromerr=5pEgJ0C1)  
[zxw394的博客](http://blog.csdn.net/zxw394/article/details/30067827)