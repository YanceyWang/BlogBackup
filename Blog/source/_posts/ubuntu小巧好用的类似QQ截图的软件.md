title: ubuntu小巧好用的类似QQ截图的软件DeepinScrot
date: 2015-07-31 09:46:52
tags: 
- Ubuntu
- 截图软件
- 随笔
toc: false
---
在Windows中当我们需要对一个图片进行复杂处理的时候，我们会选用adobe的ps软件，当我们需要快速的进行截图，然后进行简单的标记处理时，qq等类似软件提供的快速截图功能很好的满足了我们的需求。然而qq等类似软件在ubuntu中没有原生的支持，我们平常要进行截图时，就是按下PrintScreen快捷键。虽然也能达到截图目的，但总归来说没有QQ截图那么方便，任性。那今天这边介绍的一款软件可以在ubuntu中提供类似于QQ截图的功能。

<!--more-->


软件的名字是： **DeepinScrot** ，Github 地址，[点这里](https://github.com/linuxdeepin-packages/deepin-scrot).

通过下面的命令获取最新的安装包：

```PowerShell
wget http://packages.linuxdeepin.com/deepin/pool/main/d/deepin-scrot/deepin-scrot_2.0-0deepin_all.deb
```

然后，进行安装：

```PowerShell
sudo dpkg -i deepin-scrot_2.0-0deepin_all.deb
```

之后就可以使用了：

```PowerShell
deepin-scrot
```

为了使用方便，我们为它设置快捷启动键：
打开systemsetting->keybord->shortcut
![DeepinScrot](http://7xkr9a.com1.z0.glb.clouddn.com/Blog01DeepinScrotShortCut.png)

> 在custom shortcut 中新建一个项目，名字随便起。Command填  `deepin-scrot` 之后再给他个快捷键 `Ctrl+alt+A` 

看下效果，按下 `CTRL+ALT+A`:

![DeepinScrot](http://7xkr9a.com1.z0.glb.clouddn.com/Blog01DeepinScrot.png) 

很不错，几乎跟QQ截图一样，也提供了一些快速标记的功能。

感觉还不错~
