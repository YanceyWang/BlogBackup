title: Ubuntu下为软件创建快捷方式
date: 2015-08-04 15:58:22
tags:
- 随笔
- Ubuntu
- 快捷方式
toc: false
---
ubuntu/linux mint上有很多软件的安装不是直接通过APT管理器从仓库中安装的,这样就会有一个问题,我们启动软件的时候只能在软件的目录通过命令启动,典型的例子是eclipse软件,我们是直接下载软件包,然后解压点击eclipse或命令行输入eclipse运行的.这样使用起来不太方便,今天就介绍一种方法,可以为这些不是通过APT安装的软件创建快捷方式,在dash中可以直接启动.

我用的是Linux mint 发行版,所以就以这个版本为基础进行演示.其实ubuntu的操作是一样的.

##所有的快捷方式都是.desktop文件##
其实我们看到的在桌面上或dash中的软件图标都是.desktop的文件,例如我们用terminal进入desktop:
```PowerShell
wyq@wyq-m81 ~/Desktop $ ls
CmdMarkdown.desktop  eclipse.desktop  goagent-gtk.desktop  google-chrome-unstable.desktop  jetbrains-studio.desktop  meld.desktop  shadowsocks-qt5.desktop  skype.desktop  SmartcardService  virtualbox.desktop
```
<!--more-->

可以很清楚的看到这些快捷图标都是.desktop文件,那这些文件存在哪呢.
有两个地方:
> - ~/.local/share/applications/  #存放只有当前用户会看到的桌面快捷方式
> - /usr/share/applications/      #存放所有用户都可以看到的桌面快捷方式图片一般sudo apt 安装的都在这

既然是文件,免不了被编辑的命运,我们用vim打开其中一个看看.这里以android studio的快捷方式为例:
```javascript
[Desktop Entry]
Version=1.0                              //软件版本
Type=Application                         //类型,目前我所知的就是Application 和Directory
Name=Android Studio                      //名称
Icon=/home/wyq/Downloads/android-studio/bin/studio.png   //快捷方式的图标
Exec="/home/wyq/Downloads/android-studio/bin/studio.sh" %f  //执行命令,命令行怎么启动,这边就怎么写
Comment=Develop with pleasure!                  //鼠标放在快捷方式上显示的字
Categories=Development;IDE;                    //这个软件的类别,之后在dash中会显示在Development类别下
Terminal=false                              //是否在终端启动
StartupWMClass=jetbrains-studio              //主要是为了避免启动多个android                                 studio的时候在launcher出现多个androidstudio 图标
```
oK,看完这个,我们写出自己的一个快捷方式应该很简单了.这边以cmdmarkdown为例:
cmdMarkdown 的linux版本没有在包仓库中,官方给的是压缩包,解压后命令行运行就可以了:
```Shell
wyq@wyq-m81 ~ $ cd cmd_markdown_linux64/
wyq@wyq-m81 ~/cmd_markdown_linux64 $ ls
CmdMarkdown  icudtl.dat  images.jpg  images.png  libffmpegsumo.so  locales  nw.pak
wyq@wyq-m81 ~/cmd_markdown_linux64 $ ./CmdMarkdown 
```
![cmd_dir](http://7xkr9a.com1.z0.glb.clouddn.com/Blog03cmd_dir.png)
图中1是为快捷方式准备的图标.2是cmdmarkdwon程序
我要为他创建桌面快捷方式.

1. 在/usr/share/applications中创建CmdMarkdown.desktop的文件:
```Shell
cd /usr/share/applications
touch CmdMarkdown.desktop
sudo vim CmdMarkdown.desktop
```
2. 编辑desktop,写入执行命令和图标
```js
[Desktop Entry]
Encoding=UTF-8
Name=CmdMarkdwon
Comment=CmdMarkdwon
Exec=/home/wyq/cmd_markdown_linux64/CmdMarkdown        //打开的命令
Icon=/home/wyq/cmd_markdown_linux64/images.png        //图标
Terminal=false
StartupNotify=true
Type=Application
Categories=Application;Office                      //程序在哪个分类下出现
```
3.查看dash,并将快捷方式加入到桌面
```Shell
sudo cp CmdMarkdown.desktop ~/Desktop   //拷贝到桌面
```
在dash中查看:
按super(也就是win键)键.在office分类下能看到CmdMarkdown啦
![menu](http://7xkr9a.com1.z0.glb.clouddn.com/Blog03md.png)
在桌面上:
![Desktop](http://7xkr9a.com1.z0.glb.clouddn.com/Blog03desktop.png)


**大功告成**

> 其实desktop entry 更详细的用法请看[这里](http://standards.freedesktop.org/desktop-entry-spec/latest/)是他的Spec,上面的几个key 已经足够我使用了.并且,有了这个方法我们就可以随意为软件创建快捷方式了.




