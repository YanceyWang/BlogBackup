title: 猴子也能看懂的UEFI下Win8+Ubuntu双系统安装(Lenovo xiaoxin V4000)
date: 2015-08-04 14:30:26
categories:
- 玩机
tags:
- BIOS
- UEFI
- MBR
- GPT
- 双系统
---

>  **前言**
上次在公司搭建好了博客，想着回家在win8上花几分钟搭建环境，当晚发第一篇，结果没想到习惯了linux后在win8上进行工作感觉寸步难行。所以干脆在家的电脑上也给装上了Ubuntu，今天记录下安装双系统的过程以及一些自己的总结。


##**让人烦躁却不得不知道的术语**##


**首先要明白,当我们按下电脑的开机键后,电脑在启动系统之前最先执行的一段程序是放在主板上的只读程序段名字叫Firmware,至于具体电脑的寄存器是怎么复位的,寄存器是怎么寻址到Firmware的,请看知乎大神的[回答](http://www.zhihu.com/question/22364502). 我们只需要知道有这段程序,并且在启动系统前会走这段路程就足够了. 目前有两种类型的Firmware,一种是我们以前经常听到的 BIOS(发音:/ˈbaɪ.ɒs/),,那还有一种就是近几年兴起的用来代替BIOS的UEFI发音(pronounced as an initialism U-E-F-I or like "unify" without the n[a])),当我们按下电脑开机键后,电脑厂商的LOGO出来的时候狂按(F2|F12|F9|del 具体各个厂商不一样,我的联想小新就是F2)键就能进入到Firmware控制界面(Firmware setup utility,),接下来我们就看下这两种Firmware**

###BIOS与UEFI###

BIOS的启动方式是通过读取执行硬盘第一个扇区的代码,之后通过这段代码定位并且执行别的代码.由于BIOS的空间限制并且BIOS只能执行16bit的代码,BIOS在使用中有很多限制.相反的,UEFI 通过加载硬盘ESP分区上的efi文件来启动系统,EFI 启动加载器程序可以利用EFI 启动服务来做很多事情,例如从硬盘读取文件.

###<span id="secureboot">Secure Boot</span>###
Secure Boot只是UEFI的一个部分。两者的关系是局部与整体的关系。
Secure Boot的目的，是防止恶意软件侵入。它的做法就是采用密钥。UEFI规定，主板出厂的时候，可以内置一些可靠的公钥。然后，任何想要在这块主板上加载的操作系统或者硬件驱动程序，都必须通过这些公钥的认证。也就是说，这些软件必须用对应的私钥签署过，否则主板拒绝加载。由于恶意软件不可能通过认证，因此就没有办法感染Boot。
这个设想是好的。但是，UEFI没规定哪些公钥是可靠的，也没规定谁负责颁发这些公钥，都留给硬件厂商自己决定。
现在，微软就是要求，主板厂商内置Windows 8的公钥。

<!--more-->
###MBR与GPT###
我们在装系统的时候有时可能会遇到这种问题:
![installError](http://7xkr9a.com1.z0.glb.clouddn.com/15/08/04installError.jpg)
**Windows无法安装到这个磁盘。选中的磁盘具有MBR分区表。在EFI系统上，Windows只能安装到GPT磁盘**
这句话的意思是说,你的磁盘以前是按着MBR方式分区的,现在你在UEFI启动方式上,想安装系统在这个磁盘上就必须将磁盘重新按照GPT方式分区.也就是说不同的启动方式,要对应不同的分区方式,系统才能正常安装.那什么是MBR什么是GPT?

MBR和GPT是两种不同的存储磁盘分区信息方式,这些信息包括每个分区的起至位置,这样你的操作系统就知道哪个扇区属于哪个分区,以及哪个分区是可以启动的.MBR 使用的是传统的BIOS 分区表,GPT使用的是UEFI分区表.MBR的限制在于磁盘只能有四个主分区.并且主打只支持2T的容量.而GPT完全没有这些限制.

那怎么知道现在的磁盘是什么方式分区的以及怎么选择分区方式呢?
这个很简单,我们用DiskGenus查看磁盘分区的时候,在菜单栏有选项,如果当前是按MBR分区,那么会出现`将磁盘分区转换为GPT分区`的选项,如果是按GPT分区的,那么`将磁盘分区转换为MBR分区`是可选的.同时,我们在对磁盘进行分区的时候可以选择分区方式,例如我要将磁盘按GPT方式分区:
![GPT分区](http://7xkr9a.com1.z0.glb.clouddn.com/15/08/04DiskDeniusPartition.jpg)
可以看到DG工具会提示我们创建ESP分区和MSR分区.这两个分区是UEFI启动方式中非常重要的分区.
> MSR分区（Microsoft Reserved Partition，缩写MSR）即Microsoft 保留 (MSR) 分区。是每个 在GUID 分区表 (GPT) 上的 Windows操作系统（windows7以上）都要求的分区。
系统组件可以将 MSR 分区的部分分配到新的分区以供它们使用。例如，将基本 GPT 磁盘转换为动态磁盘后，系统分配的 MSR 分区将被用作“逻辑磁盘管理器”(LDM) 元数据分区。

> ESP是一个独立于操作系统之外的分区，操作系统被引导之后，就不再依赖它。这使得 ESP 非常适合用来存储那些系统级的维护性的工具和数据，比如：引导管理程序、驱动程序、系统维护工具、系统备份等，甚至可以在 ESP 里安装一个特殊的操作系统（SlaTaz Linux? PuppyLinux? Win PE?）。

在我的联想电脑上利用DG查看磁盘分区:
![磁盘分区](http://7xkr9a.com1.z0.glb.clouddn.com/15/08/04DiskGeniusPartitionTable.png)
我的电脑是用的UEFI启动方式,所以肯定会有ESP 和MSR 分区.在ESP分区中有一个EFI文件,里面有三个文件夹:
> /EFI/boot
  文件夹中有一个boot64.efi 文件,上面讲过UEFI启动方式就是通过去加载这些.efi文件来启动的.那这个efi是什么时候被加载,加载什么的?
  **boot64.efi是UEFI的默认引导,他的引导是在你的系统引导之前的,所以不管你的电脑有没有装系统,装的什么系统,这个文件都会被UEFI先引导**
  
  **具体每个文件的含义,在无忧启动论坛有[讨论](http://bbs.wuyou.net/forum.php?mod=viewthread&action=printable&tid=303679)**

![Boot dir](http://7xkr9a.com1.z0.glb.clouddn.com/15/08/04BootDir.png)

> /EFI/Microsoft/Boot
在Microsoft的Boot目录下有很多内容,包含了windows系统引导启动的所有信息，非常重要，文件夹是字体和语言部分，BCD包含了windows引导开始以后的信息（例如安装Hyper-v虚拟机和恢复还原之类的就会更新里面的信息）。bootmgfw.efi 是 Windows默认引导文件


![Microsoft dir](http://7xkr9a.com1.z0.glb.clouddn.com/15/08/04MicroBootDir.png)


> <span id="efi">/EFI/ubuntu</span>
这个文件夹在没完成双系统安装前是不会出现的,我这边是安装好双系统后才截的图.这个文件夹下也有些.efi文件,那其中有一个肯定是用来引导Ubuntu的,这边就先不说是那个.等安装完我们可以用命令查看的~

![ubuntu dir](http://7xkr9a.com1.z0.glb.clouddn.com/15/08/04UbuntuBootDir.png)

___

**说了这么多,其实没有多大卵用,我们只需要知道以下几点就可以了**

- 现在的电脑主板上用于引导启动的有两种固件了`BIOS` 和`UEFI`
   
 **BIOS启动流程**
    BIOS开机自检——》读取硬盘MBR分区的主引导记录—》控制权交给引导程序-》引导程序根据安装时候的配置读取各分区记录—》根据各分区已经有的系统情况，列出启动目录—》根据用户选择，启动选择的引导文件启动用户选择的系统。
**UEFI启动流程**
    主板上的UEFI模块—》硬盘内的第一个fat分区，如果分区内有EFI这个文件目录，就根据EFI文件目录的引导文件加载各类型的驱动和引导文件，启动系统同时完成自检。（如果第一个fat分区没有EFI目录则选择第二个，如果第一块硬盘没有，择选择第二块，或者U盘以此类推）

- 两种对磁盘的分区方式`MBR`和`GPT`,和启动方式搭配为MBR+BIOS, GPT+UEFI.

准备的差不多了,我们就可以正式开始装双系统了

##**磨刀不误砍柴功,正式开始装系统**##

开始装系统之前,还是有些工具需要准备:
1. Ubuntu iso镜像文件,这个就是要安装的ubuntu系统了,[下载地址](http://www.ubuntu.com/download)
2. 一个8g 大小U盘
3. U盘启动盘制作工具[Universal USB installer](http://www.pendrivelinux.com/universal-usb-installer-easy-as-1-2-3/)

准备好了之后,我们就可以安装了:
1. 使用Universal USB installer 制作Ubuntu U盘启动盘
将U盘插入电脑,打开Universal USB installer
![USB INSTALLER](http://7xkr9a.com1.z0.glb.clouddn.com/15/08/04Universal%20USB%20Intaller.png)
2. <span id="partition">创建Ubuntu分区</span>
  我们要给ubuntu单独分出一个分区
右键我的电脑->管理->磁盘管理->右键点击一个比较他的分区->压缩卷
![shrink volume](http://7xkr9a.com1.z0.glb.clouddn.com/15/08/04shrink%20volume.jpg)
如上图,给ubuntu分了120个G,记住这个大小,在安装ubuntu的时候会用到
3. 关闭电脑的快速启动功能
控制面板->硬件和声音->电源选项->选择电源按钮和功能
![快速启动](http://7xkr9a.com1.z0.glb.clouddn.com/15/08/04quickstart1.png)
然后点击更改当前不可用的设置->把快速启动的对勾去了
![关闭快速启动](http://7xkr9a.com1.z0.glb.clouddn.com/15/08/04quicksetup2.png)
4. 关闭<span id="secure">Secure Boot</span>
[前面](#secureboot)介绍了,secure boot 能够防止恶意程序启动,保护电脑安全,但同时也不能让没有签名的系统启动,所以我们要安装多系统,首先得关闭secure boot,
重启电脑->按快捷键F2(不同机器不同按键,可以试试F2,F12,F8,DEL)进入EUFI设置界面->键盘左右箭头调到Security->上下箭头调到Secure boot->回车弹出选框选Disable
![secure boot](http://7xkr9a.com1.z0.glb.clouddn.com/15/08/04IMAG0008.jpg)
5. <span id="5">选择U盘优先启动</span>
关闭SecureBoot后,要安装ubuntu得让系统从U盘启动
左右箭头调到BOOT一栏->上下箭头调到EFI USB Device->F6 将这个启动项上调到第一位置.
![u盘启动](http://7xkr9a.com1.z0.glb.clouddn.com/15/08/04IMAG0009.jpg)
6. 保存修改,重启
在exit一栏,选择第一项save discard and exit.保存修改并重启系统.
7. 进入tryubuntu界面
重启后进入ubuntu 安装引导界面,选择地一个try ubuntu,我们把它称为ubuntu pe.
![ubuntu 安装](http://7xkr9a.com1.z0.glb.clouddn.com/15/08/04IMAG0010.jpg)
8. ubuntu分区
进入ubuntu pe界面后,点击桌面上的install ubuntu进行安装,一路next在select installation type的时候注意选择第三项Something Else
![select](http://7xkr9a.com1.z0.glb.clouddn.com/15/08/04IMAG0013.jpg)
然后,会看到磁盘列表里面有个free space大小和你在[第2步](#partition)中分配的大小是一样的.选中他,点击左下的`+`号,创建分区.
一般来说我们只需要创建3个分区
第一个分区root,按照我这个选择就行了,大小一般给20G,也就是差不多20000M,类型:primary,新分区的位置:begining of the space ,use as:选择EXT4. 挂载点选择:/(也就是root啦)
![root](http://7xkr9a.com1.z0.glb.clouddn.com/15/08/04IMAG0014.jpg)
第二个分区swap,大小不小于你的内存大小,和root分区不同的是use as 选择为swap area.
![swap](http://7xkr9a.com1.z0.glb.clouddn.com/15/08/04IMAG0015.jpg)
第三个分区home,剩下的空间都作为home吧.和Root分区一样设置,只是挂载在home上
![home](http://7xkr9a.com1.z0.glb.clouddn.com/15/08/04IMAG0016.jpg)
之后,就一路next,等着装完吧
9. 安装完成后拔下U盘,F2进入到UEFI设置界面,在boot一栏,可以看见多了一个ubuntu 启动选项,将他上调到第一
10. 重启就进入到我们熟悉了GRUB引导界面了.默认第一项是进入ubuntu,然后还有windows boot manager 也就是win8系统.我们可以上下选择进入哪个系统.

**至此ubuntu的安装就完成啦**

##事后诸葛,看看双系统的和谐共处##
###UEFI启动选项###
[在第5步](#5)中介绍把U盘设为第一启动项的时候,就看到其实这边我是已经安装好了ubuntu,现在系统的启动项包括EFI USB,ubuntu,Windows还有UEFI PXE Network 这几个启动项.在介绍ESP分区的时候看到了有几个[efi](#efi)文件,这些文件和启动项是什么关系呢?
其实我们在ubuntu下有工具可以查看:

```PowerShell
sudo efibootmgr -v
```
这个命令可以查看当前系统所有的可用启动项
![efibootmgr](http://7xkr9a.com1.z0.glb.clouddn.com/15/08/04efibootmgr.png)
可以看到现在我的系统包含编号为0001,0003,2003,0004,2001,2002的启动项,而且我当前使用的是0001也就是ubuntu启动啦.往下看,可以发现没法启动项的启动文件都列出来了
ubuntu -> /EFI/ubuntu/shimx64.efi
win boot manager -> /EFI/Microsoft/Boot/bootmgfw.efi
Lenovo recovery system ->/EFI/Microsoft/Boot/LrsBootMgr.efi

> 注:
对Ubuntu来说在[这里](#efi)我们看到,boot文件夹下有grub64.efi 和Shimx64.efi ,而且我们进入系统也是直接进入的ubuntu的grub程序界面.那为什么不是启动grub64.efi,shimx64.efi是什么东西?
其实他家看[这里](#secure)发现,我安装完双系统后是将Secure Boot打开的.前面也说了打开Secure boot 除了win8别的系统应该没法启动的,这就是shimx64.efi的作用了.
> Typically, EFI/ubuntu/grubx64.efi on the EFI System Partition (ESP) is the GRUB binary, and EFI/ubuntu/shimx64.efi is the binary for shim. The latter is a relatively simple program that provides a way to boot on a computer with Secure Boot active. On such a computer, an unsigned version of GRUB won't launch, and signing GRUB with Microsoft's keys is impossible, so shim bridges the gap and adds its own security tools that parallel those of Secure Boot.
原来shimx64.fei是可以在sercure boot打开的情况下启动grub.

###**ubuntu下挂载Windows的分区盘**###
我安装完成后第一次进入Ubuntu,可以看到win下的磁盘分区,但是没法挂载,但当我进入一次Win系统后,再次进入ubuntu发现这些盘居然可以挂载了,那这样实现ubuntu和win的磁盘共享也不成问题了.


