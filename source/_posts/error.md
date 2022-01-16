---
title: 为啥 hexo d -g 成功了打开页面为空
date: 2021-05-14
tags: react
---
## 问题 ##
我的内心是崩溃的
昨天心血来潮想写个博客
然后部署的时候发现页面空白
然后我去github的仓库里看了下
所有html页面都是0kb也是就空的
这一下子我就不淡定了
由于本地hexo s 预览都是好的

## 尝试解决问题 ##
我以为是依赖不对 删了以后重装了，结果发现没用
然后一顿google，一开始找出来的都是_config.yml配置不对啊之类的，发现也没用
然后找到了 hexo GitHub repo 的 issues，其中 关键的一个[信息](https://github.com/hexojs/hexo/issues/4267)

Update on Feb 14th, 2021: the previous reply is outdated. Hexo is compatible with Node.js 14 since version 4.2.1 released on May 14, 2020. See also #4285

所以我检查了下我的 node 版本是14.x, hexo 版本是3.9.x。是node版本太高了！！！hexo 还不支持。所以我就升级了hexo的版本到 4.2.1 及以上。

## 总结 ##
还是得多翻issue，好多解决办法都藏在issue里面~~

More info: [麦麦](https://github.com/maimai123)
