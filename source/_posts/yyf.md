---
title: hexo博客搭建（麦麦）
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
> 我发现我的yilia主题push到github尚老师丢失，下下来没有了
> 执行
* git clone https://github.com/litten/hexo-theme-yilia.git themes/yilia


More info: [麦麦](maimai123.github.io)
