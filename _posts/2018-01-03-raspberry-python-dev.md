---
layout:     post
title:      Raspberry python 开发环境的配置
subtitle:   ARM？很不适应
date:       2018-01-03
author:     Siyuan
header-img: img/in-posts/coding.jpg
catalog: 	true
tags:
    - Linux
    - python
    - geek
---

这段时间在做土石坝现场试验的研究，手里刚做了一个用 python 开发的振动检测分析软件。正巧，手里有一块寂寞的树莓派，三代 B 配置在 ARM 开发板中算高的了，自带了四个 usb 接口、wifi 蓝牙芯片都有，近半年来放在办公室，装了 cups 配合一个超便宜的惠普打印机作为无线打印机用。所以就想把软件放到树莓派上运行以下看看，如果可行的话，把树莓派扩展一下，直接做为一套设备使用岂不是 exited。尝试过程中有很多趣事（坑。。。），这段时间会抽空都更到这篇 post 里。

## 开发环境配置

我本来是习惯用 conda 的，工作用的设备上基本上第一个装的就是 mini conda，但是不知道怎么回事，树莓派的 conda 下包安装经常出问题，还没有系统默认的 python pip 好用。。按道理不应该的。所以接下来的所有操作都是在默认的 python3 中运行。

### 更换国内镜像


树莓派运行的是 raspbian 系统，一个基于 Debian 的 ARM Linux 系统。版本代号 jessie。由于众所周知的原因，将默认源更换为国内才可以顺利的进行包管理。笔者在高校，所以就选择各个大学的开源镜像站了。

和服务器上的步骤并没有什么不一样，编辑 source.list 文件：

~~~bash
sudo vim /etc/apt/sources.list
~~~

将原文件注释掉，添加新的源：

~~~
deb http://mirrors.ustc.edu.cn/raspbian/raspbian/ jessie main non-free contrib
deb-src http://mirrors.ustc.edu.cn/raspbian/raspbian/ jessie main non-free contrib
~~~

修改之后，运行 apt update 可以发现，新的源已经生效，但是更新过程中仍然有一个英国的源出现：

~~~
100% [Connecting to archive.raspberrypi.org (46.235.227.11)]
~~~

可以说是非常的皮了。这是因为树莓派有两个源，还需要继续修改另一个 raspi.list ：

~~~
sudo vim /etc/apt/sources.list.d/raspi.list
~~~

注释掉原来的文件，添加：

~~~
deb http://mirrors.ustc.edu.cn/archive.raspberrypi.org/debian/ jessie main ui
~~~

现在才算完全替换成国内的源。

### 配置 python

python 其实没有什么特殊的，系统已经自带了 python2 和 3 。

但是！

很多 pypi 包是无法安装的，你说你的头为什么这么铁！！

笔者并不是职业程序员，尝试了 pip 和 conda 之后，我基本上已经技穷了。经过一般搜索后，发现树莓派上的三方包有些可以通过 pip 安装，有些需要 sudo pip 安装，有些需要通过 sudo apt install python-samplepackage 来安装。具体是哪些包，讲真。。。不好说 。。。。

比如 numpy 可以直接 pip 安装，而 matplotlib 则需要通过 sudo apt install python-matplotlib 来安装。而有些包则需要自己编译。可能是因为三方包很多是 c 或者 cpp 写的吧。
