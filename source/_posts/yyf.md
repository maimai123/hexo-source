---
title: hexo博客搭建
date: 2018-05-07
tags: hexo
---
<font size=1>**如有转载，请注明出处。**</font>

已经在D盘D:\git\hexo下面装好了hexo 只需要
<img src="http://a3.qpic.cn/psb?/e780e515-0f41-4fd9-b116-910ac87c9a9d/HbhW5r8SCO9iyIpUE8QS7pfUEqaVShMRHWfXyjCZLSY!/b/dI8AAAAAAAAA&bo=AAOoAAAAAAADB4k!&rf=viewer_4" />
输入 hexo generate 回车
        hexo server就可以启动了
本地访问localhost:4000可以访问了
不知道github上 ，登录github账号

在hexo的安装路径D:\git\hexo下找到_config.yml
<img src="http://a3.qpic.cn/psb?/e780e515-0f41-4fd9-b116-910ac87c9a9d/K7wRFP5VSOigejKjT6vTaOVBvDqHBy1vRsN4TRTDdlw!/b/dA0BAAAAAAAA&bo=0QLWAAAAAAADByc!&rf=viewer_4" />
deploy:
  type: git
  repository: git@github.com:maimai123/maimai123.github.io.git
  branch: master


先按ctrl+c停止本地的再
输入 hexo generate 回车
        hexo deploy就可以部署了



如果部署失败 则可能是没有设置ssh
http://jingyan.baidu.com/article/a65957f4e91ccf24e77f9b11.html

ssh内容在C:\Users\Administrator\.ssh下
设置完以后要查看邮箱
ssh密码为空
部署到github上
npm install hexo-deployer-git --save
hexo  generate
 hexo deploy
bugs
在github部署完成之后，马上访问可能出现404错误，这是正常的，（最多）等待十分钟左右就可以访问了。如果还不行，那很可能是 github 发送给你的验证邮件你没有打开看，据多方反映，验证后就没问题了

同步到github上
hexo s -g
hexo d -g

#主题丢失问题
> 我发现我的yilia主题push到github上老丢失，下下来没有了
> 执行
* git clone https://github.com/litten/hexo-theme-yilia.git themes/yilia

_config.yml设置如下
```javascript
# Header

menu:
  主页: /
  所有文章: /archives/

# SubNav
subnav:
  github: "https://github.com/maimai123"
  weibo: "http://weibo.com/marsmaimai"
  zhihu: "https://www.zhihu.com/people/yu-ya-fei-27/activities"
  bilibili: "https://space.bilibili.com/57022459?from=search&seid=2536533383147677614"
  mail: "mailto:15867193547@163.com"
  #qq: "#"
  #weixin: "#"
  #jianshu: "#"
  #douban: "#"
  #segmentfault: "#"
  #acfun: "#"
  #facebook: "#"
  #google: "#"
  #twitter: "#"
  #linkedin: "#"

rss: /atom.xml

# 是否需要修改 root 路径
# 如果您的网站存放在子目录中，例如 http://yoursite.com/blog，
# 请将您的 url 设为 http://yoursite.com/blog 并把 root 设为 /blog/。
root: /

# Content

# 文章太长，截断按钮文字
excerpt_link: more
# 文章卡片右下角常驻链接，不需要请设置为false
show_all_link: '展开全文'
# 数学公式
mathjax: false
# 是否在新窗口打开链接
open_in_new: false

# 打赏
# 打赏type设定：0-关闭打赏； 1-文章对应的md文件里有reward:true属性，才有打赏； 2-所有文章均有打赏
reward_type: 2
# 打赏wording
reward_wording: '谢谢你请我吃糖果'
# 支付宝二维码图片地址，跟你设置头像的方式一样。比如：/assets/img/alipay.jpg
alipay: https://github.com/maimai123/hexo-source/blob/master/source/img/zhifu.jpg?raw=true
# 微信二维码图片地址
weixin:

# 目录
# 目录设定：0-不显示目录； 1-文章对应的md文件里有toc:true属性，才有目录； 2-所有文章均显示目录
toc: 1
# 根据自己的习惯来设置，如果你的目录标题习惯有标号，置为true即可隐藏hexo重复的序号；否则置为false
toc_hide_index: true
# 目录为空时的提示
toc_empty_wording: '目录，不存在的…'

# 是否有快速回到顶部的按钮
top: true

# Miscellaneous
baidu_analytics: ''
google_analytics: ''
favicon: /favicon.png

#你的头像url
avatar: https://wx2.sinaimg.cn/mw2000/008rgTSEly8gxtfy3wnbkj30hs0hs757.jpg

#是否开启分享
share_jia: true

#评论：1、多说；2、网易云跟帖；3、畅言；4、Disqus；5、Gitment
#不需要使用某项，直接设置值为false，或注释掉
#具体请参考wiki：https://github.com/litten/hexo-theme-yilia/wiki/

#1、多说
duoshuo: false

#2、网易云跟帖
wangyiyun: true

#3、畅言
changyan_appid: false
changyan_conf: false

#4、Disqus 在hexo根目录的config里也有disqus_shortname字段，优先使用yilia的
disqus: false

#5、Gitment
gitment_owner: false      #你的 GitHub ID
gitment_repo: ''          #存储评论的 repo
gitment_oauth:
  client_id: ''           #client ID
  client_secret: ''       #client secret

# 样式定制 - 一般不需要修改，除非有很强的定制欲望…
style:
  # 头像上面的背景颜色
  header: '#4d4d4d'
  # 右滑板块背景
  slider: 'linear-gradient(200deg,#a0cfe4,#e8c37e)'

# slider的设置
slider:
  showTags: true

# 智能菜单
# 如不需要，将该对应项置为false
# 比如
#smart_menu:
#  friends: false
smart_menu:
  innerArchive: '点我搜文章'
  friends: '友链'
  aboutme: '关于我'

friends:
  小雷的博客: http://www.leridy.pw
  麦麦的博客: https://www.marsmai.club/
  李逵的博客: https://www.lcckup.com

aboutme: 很惭愧<br><br>很久没写博客了<br>谢谢你还能来逛逛

```


More info: [麦麦](maimai123.github.io)
