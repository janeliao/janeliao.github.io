---
layout:     post
title:      如何优雅的配置 ssh-key 登录远程 Linux 服务器
subtitle:   ssh-copy-id excited！
date:       2018-01-01
author:     Siyuan
header-img: img/in-posts/coding.jpg
catalog:     true
tags:
    - Linux
---

笔者手上有国内外小鸡若干，如果采用密码登录服务器。在远程登录服务器以及在有服务器端--本地段拷贝需求时，频繁的密码输入过程略显繁琐。所以在本地机器相对固定时，ssh-key 登录方式非常方便。在配置好 ssh-key 之后，我们登录服务器或者执行 scp 操作都不需要在此输入密码了。

可能你对 ssh-key 不是很了解，但是如果你使用 github，你其实已经在使用 ssh-key 的方式简化了远程操作步骤了。不管你的平台的是 Mac还是 Linux 的各种发行版，在你使用 ssh 隧道时，都需要一个 .ssh 文件夹。一般情况下，该文件夹的内容如下：

- id_rsa 文件，私钥
- id_rsa.pub 公钥，是向互联网公开的
- Know_hosts 用户使用过的 host 信息
- authorized_keys 存放其他主机公钥的地方，和 GitHub 中管理 keys 的页面一样的道理

当然，你可以不必在意这些文件如何如何。配置 ssh-key 只需要在本地机上执行简单的两个步骤

## 1 生成 key

这一步是在你面前的本地机上操作的，只需要执行下面这条命令：

~~~bash
ssh-keygen -t rsa
~~~

意思是生成一个 -t type 为 rsa 加密的 ssh-keygen

## 2 传到远端

网上很多资料中这一步使用的是 scp 来复制文件，但是这样有很多弊端，比如你的文件权限很可能不正确。在这里我们使用一条非常优雅的命令来完成这一操作;

~~~Bash
ssh-copy-id server_user_name@server.com
~~~

在输入服务器登录密码后，ssh-key 登录就已经配置完成了。绝对不会出现任何碰壁的现象。

## 3 more

现在我们在登录远程服务器时已经不需要输入密码了，如果你想更加简化，还可以使用 alias 赋予登录命令别名。编辑 .bashrc 或者 .bash_profile 文件，添加别名。

~~~bash
alias ecs1='ssh user@server.com'
~~~

修改后再运行下面的命令使设置生效

~~~Bash
source .bash_profile
~~~

这样，只需要输入 ecs1 就可以登录到远程服务器了。