---
title: "Clojure 学习日记 —— 2基础语法"
date: 2016-01-26 16:28:30
categories: 学习笔记
tags: clojure
---
唉，觉得有点啰嗦，废话太多，写着写着就变成了写博文，我是写笔记、写笔记。
<!--more-->

### 基本概念
- 字符串；跟java的字符串一样，不过天然支持多行。
```
"hello there"
;="hello there"
"multiline strings
are very handy"
;="multiline strings\nare very handy"  
```
 - 布尔值；类似java，true和false。
空值；这个有点不同，为nil，类似java的null，python的none。
字符；clojure看起来有点怪，没有用单引号包起来，不过跟java的字符差不多。
```
\c
;=\c 
```
 - 关键字；类似概念，直接看例子。
```
(def person {:name "Sandra Cruz"
             :City "Portland, ME"
             ::lod "Demo"})
;=#'user/person
(:city person)
;="Portland, ME" 
```
关键字是要冒号开头的，双冒号开头表示所属当前命名空间，
如果双冒号开头又包含了/，表示所属特定命名空间，
这点有点懵，我估计是翻译的不好，我实际验证下。
`:name` 表示只能在当前命名空间用`(:name person)`取到值。
`::lod` 表示你可以在当前命名空间用`(::lod person)`取到值。
同时在所有命名空间，可以用`(:user/lod person)`取到值。

- 进制
 十六进制：`0xff`表示255；
 八进制：`040`表示32；
 任意进制；`BrN`，N为数字，B为进制，`16rff`表示255，最高支持36进制。
- 精度
 clojure支持任意精度的数字。
- 正则表达式
 clojure用以`#`开头的字符串当作正则表达式，在字符串中不需要加反斜杠转义。
- 注释
- 支持俩种注释，以分号`;`开头的单行注释还有以`#_`开头的形式级别的注释，可以直接注释一个形式。
- 空格和逗号
空格和逗号基本上是等价的，一般用逗号在希望提高代码可读性的时候使用，比如`map`里面
```
(create-user {:name new-username, :email email)
```
 - 集合字面量
 字面量这个看下来很高大上的词其实不是新概念，就相当于我们的数据形式，有四种。
```
`(a b :name 12.5)      ;list
['a 'b :name 12.5]     ;vector
{:name "Chas" :age 31} ;map
#{1 2 3}               ;set
```