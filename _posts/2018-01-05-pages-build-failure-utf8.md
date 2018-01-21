---
layout:     post
title:      Github pages build failure
subtitle:   File is not properly UTF-8 encoded? 也许这里可以帮到你
date:       2018-01-05
author:     Siyuan
header-img: img/in-posts/coding.jpg
catalog: 	true
tags:
    -  github pages
    -  python
    -  encoding
---

Github pages 绝对是一项超值服务，如果没有动态网页要求的话，已经可以把这项服务当做一个不错的网站虚拟机了。很多托管在 github 上的优秀的项目都会建立一个相应的 github pages 页面。具体的建立流程十分简单，笔者就不在此赘述了。今天给大家分享一个 buil failure 问题的解决方法。

这几天对一些国内免费的开源 api 很感兴趣，建了一个 repo http://github.com/siyuanxu/toys 。大体上就是用 python 聚合一些信息写成 markdown 文件，并使用 github pages 做一个简单的静态页面。项目很简单，pages 上线也很顺利，但是上午在办公室更新了一下数据，竟然收到了 github 发的 build failure 邮件，提示内容是 File is not properly UTF-8 encoded。想了一下原因应该是办公室的 windows 系统中输出文本的编码有问题，而私人用的 MacBook 则没有此问题。在网上搜了一下，有一个解决办法是使用 iconv.exe 来进行转码，大体如下：

~~~bash
iconv -f GBK -t utf-8 origin.md > new.md
~~~

但是试了之后疗效并不好，当然有可能是因为我原来的文件并不是 GBK，而且这个方法不太 python。所以还是想在 python 中进行修改。简单搜索后有方法如下：

原来的文本写操作为

~~~python
with open('index.md','w') as f:
	f.write(content)
~~~

修改后为

~~~python
import codecs

with codecs.open('index.md','w','utf-8') as f:
	blah blah blah
~~~

codes 是 python 的自带库，经过 codecs 内部函数操作的文本可以对编码格式进行指定。经过这样修改后，在此运行代码并提交至 github，build 成功。

以上。

