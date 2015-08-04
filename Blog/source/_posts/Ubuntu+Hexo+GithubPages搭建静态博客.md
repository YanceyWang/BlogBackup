title: Hexo+Ubuntu+GithubPages搭建个人博客(基础篇) 
date: 2015-07-30 11:25:44
tags: [Hexo,Ubuntu,github]
categories:
- 搭建博客
---


> **前言**
最近发生了一些事情，不知道该怎么面对，想让自己忙起来，没有时间去想别的事情。同时，让我对过去进行了一个反思，以及对将来进行了初步的规划，我想改变，也一定会改变，借此博客，记录我成长的点点滴滴。希望我希望看到的人能看到，也希望能给大家带来点帮助。

第一篇博客就用来记录下我搭建这个独立博客的经过吧

##**准备**##

###**静态博客和动态博客**###

所谓静态博客，它是在写好文章后，在本地生成静态页面后再上传到服务器（Jekyll在Github Page上的例子有点特殊，后面会讲到），而不是像传统的Wordpress等基于动态语言、数据库博客那样在访问的时候从数据库读取数据动态生成页面。静态博客最大的优点是速度快，对网站使用的空间要求也不高，想找一个不支持静态页面的空间也是挺难的，缺点嘛，功能没有Wordpress等传统博客那么强。静态博客的”静态”主要体现在浏览者的角度，而不是写作者的角度，其最终的HTML文件，也是计算机生成的。静态博客与动态博客的差别就好像去超市里买一罐咖啡与去咖啡厅点一杯咖啡的差别。

在静态博客出现之前，更多的人选择的是传统的动态博客。传统的动态博客一般有2种常见的方法：

 > * 自建博客网站：需要了解数据库、HTML/CSS和PHP等很多Web编程知识
 > * 注册公共博客：一般要受制于公共博客服务提供商的博客创作工具的写作效率

实际上大部分博主写博客仅仅是个人性质的，文章数量不多，流量也不大，所以并不需要自己建站那么复杂。但如果注册公共博客，又会非常依赖服务提供商所提供的主题、功能和写作工具，缺少自由度。静态博客就可以很好的弥补两者的缺点。静态博客和传统博客最大的区别就在于写作与发布的过程有所不同。

静态博客首先会在本地进行离线写作，然后通过工具将离线本地作品转化为HTML网页发布到在线网站上
动态博客首先要建立网站（当然也可以注册一个公共博客服务），然后在线完成博客的编辑和发布
静态博客可以让博主更专注于写作，而不是花费很多精力在建站技术和写作工具上：

 > * 博主不需要折腾各种建站技术，任何支持静态网页的主机空间和云存储空间都可以拿来发布博客
 > * 博主可以自由选择自己喜欢的本地写作工具，而不需要受制于在线博客写作工具的质量火热用户体验

<!--more-->

###**GitHub 与 静态博客**###

静态博客是个新生事物。静态博客的流行跟著名的开源社区Github的支持是分不开的。因此，绝大部分静态博客平台设计得还是比较极客，更适合有一定编程技能的人来使用，并不适合完全零起点的完全没有编程知识的普通用户。而Github除提供在线Markdown编辑器之外，还提供了Github Page服务，可以将用户托管在Github上的Markdown博客发布为静态网站。博主完全可以将自己的博客站免费寄放在Github上。此外，通过Github发布博客，还有一个额外的好处，那就是你的博客文章可以用Git来管理，这样你就可以在Github上获得博客文章的完整发布历史（像程序员管理源代码一样），并且可以随时获取历史版本或回退和回退到特定的版本上，相当于有了强大的备份系统。

###**Hexo**

hexo出自台湾大学生tommy351之手，是一个基于Node.js的静态博客程序，其编译上百篇文字只需要几秒。hexo生成的静态网页可以直接放到GitHub Pages，BAE，SAE等平台上。

**本着不花钱，爱折腾的原则，本人选择Hexo加上Git Pages 的方法搭建自己的独立博客**

##**开始**

###**Hexo环境安装**
> 在Hexo的[官网](https://hexo.io/docs/)详细介绍了Hexo的安装和使用，安装Hexo的前提是需要先安装Node js和Git。

####**Ubuntu 安装NodeJS**

在ubuntu上安装Node.js也有很多方法，最常见的有两种：一种是通过Node.js的[官网](https://nodejs.org/)下载源码编译安装,另一种就是按照Hexo官网介绍的方法安装,这边以第二中方式为例.

* 下载和安装nvm,nvm介绍[请看](http://weizhifeng.net/node-version-management-via-n-and-nvm.html):

```PowerShell
#这边可以使用curl也可以通过wget
$ curl https://raw.github.com/creationix/nvm/master/install.sh | sh
#不过,我只有wget方法能成功
$ wget -qO- https://raw.github.com/creationix/nvm/master/install.sh | sh
```

* nvm安装好了之后,通过nvm安装node.js

```PowerShell
#0.12 为node.js版本,Current Version: v0.12.7
$nvm isntall 0.12
```
至此Nodejs安装完毕,我们可以通过命令查看nodejs 版本

```PowerShell
wyq@wyq-m81 ~ $ node -v
v0.10.25
```
####**Ubuntu安装Git**
在ubuntu中安装git还是相当简单的,直接使用包管理器进行安装就好:

```PowerShell
$sudo apt-get install git
$sudo apt-get install git-core
```

**Nodejs 和 git都已经安装好了,接下来就可安装我们的主角Hexo了**

```PowerShell
$ npm install -g hexo-cli
```
安装过程有点慢,这个时候你可以问候一下GFW.

###**Github pages 仓库创建**

安装好Hexo后,就该另一个主角Github pages 登场了.关于Github pages的介绍,请看[这里](https://pages.github.com/),它主要的作用是github给你或你的团队展示你的项目的地方,我们用这个来放我的博客简直再适合不过了.
![new repo](http://7xkr9a.com1.z0.glb.clouddn.com/Blog00NewRepo.png)
![new repo](http://7xkr9a.com1.z0.glb.clouddn.com/Blog00GithubIO.png)



Github pages本质还是你github上的一个repository,只不过它的命名规则是[suername].github.io的形式,并且每个github只能有这么一个这样的仓库.创建好这样的仓库后就可以在浏览器地址栏输入[username].github.io进行访问了.



##**执行**

> Hexo和Github pages 准备好以后,我们就可以正式开始博客建立了.

###**初始化本地博客目录**

```PowerShell
#在home新建一个文件夹,用于本地存放博客
$mkdir Blogs
$cd Blogs
#用hexo命令在Blogs目录中初始化环境
$hexo init
$npm install
```
> 这个时候,其实Hexo为我们生成了一个默认的名为HelloWorld的一篇博客,接下来我们看下这篇博客

###**生成静态文件**

```PowerShell
#使用hexo generate命令可以将我们编写的markdown文件转换成html文件
$hexo generate
```
###**本地服务开启**

```PowerShell
wyq@wyq-m81 ~/Blog $ hexo server
INFO  Hexo is running at http://0.0.0.0:4000/. Press Ctrl+C to stop.
```
> 这个时候在浏览器地址栏输入localhost:4000就可看到Hexo为我们生成的这边博客了
大概就是这个样子(默认的标题和名字已经被我改掉啦)

![默认HellowordPost](http://7xkr9a.com1.z0.glb.clouddn.com/Blog00HelloWorld.png)

> 到此我们已经把基本建博客的流程走过一遍了,怎么样,是不是很easy呀.接下来我们完成三个事情:创建一个篇新的博客,给博客换个主题,上传到Github pages.

###**创建新的博客**

```PowerShell
#使用hexo new [postname]新建一篇博客
$hexo new firstblog
#完成之后,进入到source/_psots目录就可以看到我们刚才新建的博客了
$cd source/_posts
$ll
drwxr-xr-x 2 wyq wyq 4096 Jul 30 11:25 .
drwxr-xr-x 3 wyq wyq 4096 Jul 29 16:17 ..
-rw-r--r-- 1 wyq wyq   53 Jul 30 11:25 firstBlog.md
-rw-r--r-- 1 wyq wyq  852 Jul 29 18:04 HelloWorld.md
#firstBlog.md就是我们刚才新建的一篇博客
```

博客文章都是md格式的markdown文件,我们打开来看下
```PowerShell
#Hexo默认为我们生成了三行
title: firstBlog #这边博客的标题
date: 2015-07-30 11:25:44 #博客创建时间
tags: #博客的tag
```
那我们只需要使用markdown语法在这个文件里面写我们的博客就ok了,这里推荐在线markdwon编辑器,[CMD markdown](https://www.zybuluo.com),至于markdown语法,我们只需要了解常用的就行了,学习成本几乎等于0,[Github markdown语法](https://help.github.com/articles/markdown-basics/)

###**更改博客主题**
Hexo 有丰富的主题支持,这也是我选择它的原因之一,在[这里](https://github.com/hexojs/hexo/wiki/Themes)列出了所有的Hexo 主题,大家可以选择自己喜欢的使用.我这里以Yilia为例

在我们使用`hexo init` 初始化本地博客环境后,会看到如下的文件目录结构:

```PowerShell
.
├── _config.yml
├── package.json
├── scaffolds
├── scripts
├── source
|   ├── _drafts
|   └── _posts
└── themes
```
这边先直介绍其中几个:

* _config.yml文件整个博客的配置文件,
* source/_posts文件夹包含了当前整个博客的所有文章
* themes 文件夹包含了当前的主题文件.

我们要使用[Yilia](https://github.com/litten/hexo-theme-yilia)主题
* 在当前目录(这里就是Blogs)中,clone Yilla 代码

```PowerShell    
$ git clone https://github.com/litten/hexo-theme-yilia.git themes/yilia
```

*更改_config.yml文件,在最后加入使用主题语句

```PowerShell
theme: yilia
```

* 进入到主题目录,编辑主题配置文件

```PowerShell
$cd themes/yilia
$vim _congif.yml
```
主题文件的配置说明

```JSON
# Header
menu:
  主页: /
  所有文章: /archives
  # 随笔: /tags/随笔

# SubNav
subnav:
  github: "#"
  weibo: "#"
  rss: "#"
  zhihu: "#"
  #douban: "#"
  #mail: "#"
  #facebook: "#"
  #google: "#"
  #twitter: "#"
  #linkedin: "#"

rss: /atom.xml

# Content
excerpt_link: more
fancybox: true
mathjax: true

# Miscellaneous
google_analytics: ''
favicon: /favicon.png

#你的头像url
avatar: ""
#是否开启分享
share: true
#是否开启多说评论，填写你在多说申请的项目名称 duoshuo: duoshuo-key
#若使用disqus，请在博客config文件中填写disqus_shortname，并关闭多说评论
duoshuo: true
#是否开启云标签
tagcloud: true

#是否开启友情链接
#不开启——
#friends: false
#开启——
friends:
  奥巴马的博客: http://localhost:4000/
  卡卡的美丽传说: http://localhost:4000/
  本泽马的博客: http://localhost:4000/
  吉格斯的博客: http://localhost:4000/
  习大大大不同: http://localhost:4000/
  托蒂的博客: http://localhost:4000/

#是否开启“关于我”。
#不开启——
#aboutme: false
#开启——
aboutme: 我是谁，我从哪里来，我到哪里去？我就是我，是颜色不一样的吃货…
```

###**生成静态文件,在本地预览**
```cpp
$hexo generate
$hexo server
```
效果如下
![use yilia theme](http://7xkr9a.com1.z0.glb.clouddn.com/Blog00UseYiliaTheme.png)

**ok,大功告成,就差把博客部署到github.io了

##**将博客部署到github pages中**
在博客配置文件_config.yml中,有个deployment一节,在这边我们进行github pages 的配置

```PowerShell
# Deployment
## Docs: http://hexo.io/docs/deployment.html
deploy:
  type: git #注意,这边是git不是github,hexo 3.0以后就要求使用git,不然会出现deploy失败情况.
  repo: https://github.com/YanceyWang/YanceyWang.github.io
  branch: master
```

配置好了之后,就可以吧我们博客deploy到github 上啦.

```PowerShell
$hexo clean
$hexo generate
$hexo deploy
```

之后,在地址栏输入[username.github.io] 就可以访问你的博客了.

##**使用7牛图床**

考虑到博客的速度，同时也为了便于博客的迁移，图床是必须的。我墙裂推荐七牛，访问速度极快，支持日志、防盗链和水印。

免费用户有每月10GB流量+总空间10GB+PUT/DELETE 10万次请求+GET 100万次请求，这对个人博客来说足够，不够的话点这个活动页面，也可通过邀请好友获得奖励，我也求一下七牛邀请。有一点要说明的是，七牛没有目录的概念，但是文件名可以包含/，比如2013/11/27/reading/photos-0.jpg，参考这里关于key-value存储系统。

* **首先要去7牛[官网](http://www.qiniu.com/)注册帐号,然后准备好身份证前后照,进行身份验证,不然很多功能无法使用.**
验证好了大概就是这个样子
![身份验证](http://7xkr9a.com1.z0.glb.clouddn.com/Blog007niuIDvER.png)

* **接着就可以创建空间了**
![创建空间](http://7xkr9a.com1.z0.glb.clouddn.com/Blog00CreateRepo.png)

* **然后进行图片上传,获取外连**
![获取外连](http://7xkr9a.com1.z0.glb.clouddn.com/Blog007niuouterlink.png)

* **还可以新建样式,给图片设置缩放,加水印等.**
![新建样式](http://7xkr9a.com1.z0.glb.clouddn.com/Blog00CreateStyle.png)



> **后记**
今天就写到这吧,本来昨天在公司已经配置的差不多了,以为回家能够轻车熟路的在windows上弄好环境,结果是我想多了.在ubuntu上用习惯了命令,在win上真是痛苦啊,想打开终端,`CTRL+ALT+T`,结果才发现是在win上,想看目录`ll`,结果提示没有这个命令..一气之下下载了Ubuntu 14.04 给PC装了双系统. 
第一次写博客,肯定有很多不足之处,让大家见笑了,希望您提出您的宝贵意见,我会努力做的越来越好.
这篇文章只是Hexo搭博客的基础篇,可能会有进阶篇,会介绍域名购买,评论系统使用以及更多的一些优化.

