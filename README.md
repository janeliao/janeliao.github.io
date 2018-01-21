# Siyuan blog

## unsplash api

[unsplash](https://nsplash.com) 可以说是我用过的最好的图库，照片质量上乘，API 接口丰富。所以，我计划使用 source api 来作为我的随机图片接口。

在注册了一个 unsplash 账号 account 'siyuan_4_unsplash (with pwd 10****26)' 之后，我手动挑选出了一个 collection
Unsplash api was used in this blog to set fresh head imgs. What I used here is a random collection source. 'for blog', with a **collection num 1460080** 。

那么通过下面这一行简短的代码，就可以得到图库中随机的一张图片。

```
https://source.unsplash.com/collection/1460080
```


## 阿里云 CDN 加速

为了优化站点在国内的访问速度，我选择使用阿里云 CDN 进行中间加速服务。具体实现过程如下：

- 完成 githubpages 的基本操作
- 使用有备案的域名映射 CNAME www.siyuanxu.com 到 siyuanxu.github.io ，这里的线路选择为海外。此时我们还不能在国内访问到 www.siyuanxu.com ，而通过海外线路映射，海外访问则可以直接链接到 github 的 dns。
- 开通 CDN 服务，CDN 配置如下：
    - 加速网站：www.siyuanxu.com
    - 业务类型：图片及小文件
    - 源站设置：siyuanxu.github.io:80
- 审核通过后，复制 CDN 的 CNAME www.siyuanxu.com.w.kunlungr.com
- 在 DNS 解析中添加 CNAME 解析 www.siyuanxu.com 到 www.siyuanxu.com.w.kunlungr.com ，线路选择为默认

等待以上设置生效之后，githubpages 站点在国内和国外均能达到最佳速度。

**注意**：

- 域名须备案，如果不备案，国内的 CDN 供应商会把你的 CDN 节点布置在海外，这样我们设置 CDN 就没有什么意义了。



## Change log

- 0.1 Blog running


