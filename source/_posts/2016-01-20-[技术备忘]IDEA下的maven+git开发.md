---
title: IDEA 下的 maven+git 开发
date: 2016-01-20 12:23:11
categories: 技术备忘
tags: [IDEA, maven, git]
---
不错不错，有了maven开发就简单多了，不会出现那么多莫名其妙的结构。
<!--more-->
### 为什么要用maven？
maven是一个项目管理工具，基于Java平台，主要用于项目构建，依赖管理等。  
可以说在企业开发中这个是必不可少的，这样说可能有些空泛，给你们列点实际点的。  


>`1. 在进行ssh开发的导jar包的时候是不是很痛苦，经常会遇到jar包冲突的情况？`  
>`2. 团队开发还必须统一下开发环境，考虑用idea还是myeclipse，因为其不同的项目文件？`  
>`3. 团队开发是不是很纠结，共享代码要复制整个jar包库？ `  


嗯，是不是若有所思了呢？  
那就好，接下来我们用maven来解决这些问题  

***

### 安装maven和配置环境变量

因为我的电脑并没有手动装maven，用的是myeclipse集成的maven，  
大概看了下网上的教程，基本无脑安装，各位可以自行安装，  
要注意的一点是如果需要用到命令行的就记得添加环境变量，  
我是没有添加的，因为idea插件对maven支持的很好，不需要我们做什么命令行操作了。  

+ [maven官网](http://maven.apache.org/)  
+ [maven教程网](http://tutorialspoint.com/maven/)  

上面的maven官网就不用说什么了，在这里下载，至于这个教程网，介绍的比较详细，可以参考下  

![down](/img/pics/2016-01-20/maven.png)


***


### 创建git项目

在这里我使用的是coding这个代码托管平台，在这里创建项目，其他的平台也可以，这个没什么区别  

![down](/img/pics/2016-01-20/maven2.png)  

![down](/img/pics/2016-01-20/maven3.png)  

复制下https的git访问链，等下要用到。  

***

### 在IDEA应用git项目

#### 导入git项目

![down](/img/pics/2016-01-20/idea.png)  

![down](/img/pics/2016-01-20/idea2.png)  

将我们刚才复制的git链粘贴进去，然后ok，这个项目就是git项目了，你可以pull or push来进行更新。  

#### 添加.gitignore

如我们所知，idea的项目的根目录有个.idea的文件夹，myeclipse有.setting文件夹，  
这些IDE的配置文件肯定不能上传到托管平台的，所以在.gitignore里面要加入  

```markdown
# Maven
target/
*.ser
*.ec

# IntelliJ Idea
.idea/
out/
*.ipr
*.iws
*.iml

# Eclipse
.classpath
.project
.settings/
.metadata/
```


这样这些配置文件就被git忽略掉了，记得，在idea中要确保.gitignore的颜色是绿色的，  
才表示加入到git中未提交的，红色是没有加入，蓝色是修改过的，如下图右键点击文件可以加入。  

![down](/img/pics/2016-01-20/idea3.png)  

#### 在IDEA中添加maven支持  

右键项目，有个`Add Framework support`，点击进去可以看到如下  

![down](/img/pics/2016-01-20/idea4.png)  

选择maven后结束，这样就把这个项目变成maven项目了  
可以看到项目多出了一个项目骨架及`pom.xml`文件  
记得加入git中，这样生成的骨架是默认的，如果需要就在`pom.xml`中添加支持，自行创建文件夹  

这里我以添加ssh框架支持做个小例子  

```java
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.gank.sandCrossFingers</groupId>
  <artifactId>BackEndSystem</artifactId>
  <packaging>war</packaging>
  <version>1.0-SNAPSHOT</version>
  <name>BackEndSystem Maven Webapp</name>
  <url>http://maven.apache.org</url>
  <build>
    <plugins>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-compiler-plugin</artifactId>
        <!--如果要使用jdk1.8，需要spring4搭配，否则版本有冲突-->
        <configuration>
          <source>1.7</source>
          <target>1.7</target>
        </configuration>
      </plugin>
    </plugins>
  </build>
  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
  </properties>
  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>3.8.1</version>
      <scope>test</scope>
    </dependency>
    <dependency>
      <groupId>commons-dbcp</groupId>
      <artifactId>commons-dbcp</artifactId>
      <version>1.2.2</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-webmvc</artifactId>
      <version>3.2.2.RELEASE</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-web</artifactId>
      <version>3.2.2.RELEASE</version>
    </dependency>
    <dependency>
      <groupId>c3p0</groupId>
      <artifactId>c3p0</artifactId>
      <version>0.9.1.2</version>
      <type>jar</type>
      <scope>compile</scope>
    </dependency>
    <!--
    <dependency>
        <groupId>org.slf4j</groupId>
        <artifactId>slf4j-api</artifactId>
        <version>1.6.1</version>
        <type>jar</type>
        <scope>compile</scope>
    </dependency>
     -->
    <dependency>
      <groupId>log4j</groupId>
      <artifactId>log4j</artifactId>
      <version>1.2.16</version>
      <scope>compile</scope>
    </dependency>
    <dependency>
      <groupId>org.slf4j</groupId>
      <artifactId>slf4j-log4j12</artifactId>
      <version>1.6.1</version>
      <type>jar</type>
      <scope>compile</scope>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-orm</artifactId>
      <version>3.2.2.RELEASE</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-aspects</artifactId>
      <version>3.2.2.RELEASE</version>
    </dependency>

    <dependency>
      <groupId>org.apache.struts</groupId>
      <artifactId>struts2-spring-plugin</artifactId>
      <version>2.3.1.2</version>
    </dependency>


    <dependency>
      <groupId>org.apache.struts</groupId>
      <artifactId>struts2-json-plugin</artifactId>
      <version>2.3.1.2</version>
    </dependency>

    <dependency>
      <groupId>org.hibernate</groupId>
      <artifactId>hibernate-core</artifactId>
      <version>3.6.10.Final</version>
    </dependency>
    <dependency>
      <groupId>javassist</groupId>
      <artifactId>javassist</artifactId>
      <version>3.12.1.GA</version>
    </dependency>
    <dependency>
      <groupId>mysql</groupId>
      <artifactId>mysql-connector-java</artifactId>
      <version>5.1.25</version>
    </dependency>
    <dependency>
      <groupId>org.apache.tomcat</groupId>
      <artifactId>servlet-api</artifactId>
      <version>6.0.33</version>
      <type>jar</type>
      <scope>provided</scope>
    </dependency>
    <dependency>
      <groupId>org.codehaus.jackson</groupId>
      <artifactId>jackson-mapper-asl</artifactId>
      <version>1.9.3</version>
      <type>jar</type>
      <scope>compile</scope>
    </dependency>
    <dependency>
      <groupId>org.codehaus.jackson</groupId>
      <artifactId>jackson-core-asl</artifactId>
      <version>1.9.3</version>
      <type>jar</type>
      <scope>compile</scope>
    </dependency>
    <dependency>
      <groupId>commons-lang</groupId>
      <artifactId>commons-lang</artifactId>
      <version>2.6</version>
      <type>jar</type>
      <scope>compile</scope>
    </dependency>
    <dependency>
      <groupId>org.apache.velocity</groupId>
      <artifactId>velocity</artifactId>
      <version>1.7</version>
    </dependency>
    <dependency>
      <groupId>javax.servlet</groupId>
      <artifactId>jstl</artifactId>
      <version>1.2</version>
      <type>jar</type>
      <scope>compile</scope>
    </dependency>
    <dependency>
      <groupId>commons-beanutils</groupId>
      <artifactId>commons-beanutils</artifactId>
      <version>1.8.3</version>
    </dependency>
  </dependencies>
</project>

```

修改完pom.xml文件后右上角可能是出现这个  

![down](/img/pics/2016-01-20/idea7.png)  

点击后，每次修改`pom.xml`代码就会自动提交更新了，会自动根据`pom.xml`里面的配置下载jar包  
在团队协作中，只要这个`pom.xml`文件一样，就可以保证你们的jar包完全一样，代码托管也不必上传一大堆jar包,  
导入maven项目后就会自动根据`pom.xml`文件下载的  

点击右边边界的maven，可以弹出个窗口，可以查看maven的详细信息  

![down](/img/pics/2016-01-20/idea8.png)  

如果要查看jar包冲突，可以点击这个去查看排除，有冲突会提示，只要`pom.xml`写得对一般没问题，这里就不多说了。  

![down](/img/pics/2016-01-20/idea9.png)  

#### 添加webapp文件夹

前面已经说了，我们添加的骨架只是默认的，一般web的maven项目在`/src/main`下有个`webapp`目录，  
里面存放`web.xml`等文件，没关系，我们可以自行创建文件夹，创建完`webapp`可以看到图标发生变化，  
标明这是个web目录，如果没有变也没关系，可能是默认设置有误，我们可以去项目设置那里去设置，如下  

![down](/img/pics/2016-01-20/idea10.png)  

![down](/img/pics/2016-01-20/idea11.png)  

![down](/img/pics/2016-01-20/idea12.png)  

> `注意上下两个路径都要仔细看清楚，不要选错，这个很容易错！！！`  

然后好了，基本上就是这样了，接下来就是ssh配置文件的添加，类似添加`web.xml`，然后添加服务器运行，完了。  

水平所限，欢迎指点交流，谢谢。  

### 参考
[Judas.n的教程](http://www.youmeek.com/intellij-idea-part-xviii-maven/)  
[Golphing的博客](http://blog.csdn.net/gol_phing/article/details/49331845)