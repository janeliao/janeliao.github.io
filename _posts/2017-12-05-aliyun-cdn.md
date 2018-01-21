---
layout:     post
title:      使用 aliyun cdn 加速 githubpages
subtitle:   githubpages 搭建好了但是访问速度不理想？不会再有啦！
date:       2017-12-05
author:     Siyuan
header-img: img/in-posts/coding.jpg
catalog:     true
tags:
    - cdn
    - web
---

由于笔者在高校工作，网络环境基本在教育网里面，所以访问 github 的速度还是有点不理想，当然，国内其他网络环境访问其实也好不到哪里去。为了优化站点在国内的访问速度，我选择使用阿里云 CDN 进行中间加速服务。具体实现过程如下：

- 完成 githubpages 的基本操作
- 使用有备案的域名映射 CNAME www.siyuanxu.com 到 siyuanxu.github.io ，这里的线路选择为海外。此时我们还不能在国内访问到 www.siyuanxu.com ，而通过海外线路映射，海外访问则可以直接链接到 github 的 dns。
- 开通 CDN 服务，CDN 配置如下：
    - 加速网站：www.siyuanxu.com
    - 业务类型：图片及小文件
    - 源站设置：siyuanxu.github.io:80
- 审核通过后，复制 CDN 的 CNAME www.siyuanxu.com.*****.com
- 在 DNS 解析中添加 CNAME 解析 www.siyuanxu.com 到 www.siyuanxu.com.*****.com ，线路选择为默认

等待以上设置生效之后，githubpages 站点在国内和国外均能达到最佳速度。