---
title: Hexo 配置小记
date: 2016-04-01 00:06:16
tags: [Hexo]
categories: 技术
---

## 域名解析
[dnspod](http://www.jianshu.com/p/1d427e888dda)

## 404页面
[配置大全](http://ibruce.info/2013/11/22/hexo-your-blog/)

## 图床

<!--more-->

配置方法參考上一節
[七牛](https://portal.qiniu.com/)
![](http://7xshxx.com1.z0.glb.clouddn.com/qiniu.jpg?imageView/2/w/100/)

## 阅读统计


[旧版配置](http://notes.xiamo.tk/2015-10-21-%E4%B8%BANexT%E4%B8%BB%E9%A2%98%E6%B7%BB%E5%8A%A0%E6%96%87%E7%AB%A0%E9%98%85%E8%AF%BB%E9%87%8F%E7%BB%9F%E8%AE%A1%E5%8A%9F%E8%83%BD.html#%E9%85%8D%E7%BD%AELeanCloud)
新版配置： 直接在NexT主题的根目录下的_config.yml文件中找到对应配置项，修改如下：

     leancloud_visitors: 
         enable: true 
         app_id: #<AppID> 
         app_key: #<AppKEY>

## 站点统计
[不蒜子](http://ibruce.info/2015/04/04/busuanzi/)

## 字数统计
[参考](http://blog.willin.wang/posts/2015/hexo-wordcount/)
补充说明： 在theme的languages的zh-Hans.yml文件中找到post项，增加
    
    wordcount: 字数

然后在layout/macro/post.swig中合适的位置放置如下代码：

    <span class="post-count">  |  {{__('post.wordcount')}}{{ wordcount(post.content) }}</span>

## 其他配置
[Next配置参考](http://theme-next.iissnan.com/theme-settings.html)
