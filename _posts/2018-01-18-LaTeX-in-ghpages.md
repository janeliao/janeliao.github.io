---
layout:     post
title:      LaTeX equation in ghpages
subtitle:   使用 mathjax 引擎渲染 github pages 中的公式
date:       2018-01-18
author:     Siyuan
header-img: img/in-posts/algorithm.jpg
catalog: 	true
tags:
    -  web
    -  LaTeX
    -  MarkDown
---



使用 GitHub pages 服务有一段时间了，体验很棒。不过在上一篇 [蒙特卡洛随机算法 101 ](http://www.siyuanxu.com/2018/01/11/monte-carlo/) 的发布过程中发现，GitHub 的 markdown 语法并不支持 LaTeX 公式的渲染，所以今天晚饭后抽了几分钟时间搜索了以下解决方案，在这里分享出来。

GitHub pages 使用的时 jeklly 程序，在主题文件中是支持 js script 的。所以在博客的 repo 中找到

~~~bash
_includes/head.html
~~~

然后添加

~~~html
<script type="text/x-mathjax-config">
  MathJax.Hub.Config({
    extensions: ["tex2jax.js"],
    jax: ["input/TeX", "output/HTML-CSS"],
    tex2jax: {
      <!--$表示行内元素，$$表示块状元素 -->
      inlineMath: [ ['$','$'], ["\\(","\\)"] ],
      displayMath: [ ['$$','$$'], ["\\[","\\]"] ],
      processEscapes: true
    },
    "HTML-CSS": { availableFonts: ["TeX"] }
  });
 </script>
 <!--加载MathJax的最新文件， async表示异步加载进来 -->
 <script type="text/javascript" async src="https://cdn.mathjax.org/mathjax/latest/MathJax.js">
 </script>
~~~

这样就可以载入一个配置好的公式渲染引擎了，单个 \$ 表示行内，\$$ 表示公式块。和我常用的 typora 编辑器的渲染方式一样。
$$
P=\sum^n_1\sigma_i+Excited!!
$$
