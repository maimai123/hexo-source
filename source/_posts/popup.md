---
title: jquery 简单弹出框插件教程（麦麦）
---
简单的弹出框~(目前一个页面只允许有一个弹出框，多个dom会影响，以后多多改进)
<font size=1>**如有转载，请注明出处。**</font>
><i class="icon-cloud"></i>弹出框htm部分
<img src="https://github.com/maimai123/maimai123.github.io/blob/master/img/5.png?raw=true" />
<img src="https://github.com/maimai123/maimai123.github.io/blob/master/img/65.png?raw=true" />
<img src="https://github.com/maimai123/maimai123.github.io/blob/master/img/36.png?raw=true" />
 ```python
 <!DOCTYPE html>
 <html lang="en">
 <head>
   <meta charset="UTF-8">
   <title>dialog弹出框</title>
   <link rel="stylesheet" href="../build/css/a.css"/>
   <script src="http://www.lanrenzhijia.com/ajaxjs/jquery.min.js"></script>
   <script type="text/javascript" src="../build/js/a.js"></script>

 </head>
 <body>
   <button id="btn">弹出框</button><button id="btn1">弹出框</button>
   <form id="formAdd" class="hide">
     <div class="form-group">
       <label for="exampleInputEmail1">Email address</label>
       <input type="email" class="form-control" id="exampleInputEmail1" placeholder="Email">
     </div>
     <div class="form-group">
       <label for="exampleInputPassword1">Password</label>
       <input type="password" class="form-control" id="exampleInputPassword1" placeholder="Password">
     </div>
     <div class="form-group">
       <label for="exampleInputFile">File input</label>
       <input type="file" id="exampleInputFile">
       <p class="help-block">Example block-level help text here.</p>
     </div>
     <div class="checkbox">
       <label>
         <input type="checkbox"> Check me out
       </label>
     </div>
     <button type="submit" class="btn btn-default">Submit</button>
   </form>
   <script type="text/javascript">
     $(document).ready(function(){
       $("#btn").dialog({
         width :500,
         // height:120,
         title : "提示信息",
         //content :"我在测试哦",
         contentDom :"#formAdd"
       });
     });
   </script>
 </body>
 </html>
```
<!--more-->
><i class="icon-cloud"></i>弹出框插件
> css部分
.l{float:left}.r{float:right}.cl{clear:both}.n{font-weight:normal;font-style:normal}.b{font-weight:bold}.i{font-style:italic}.fw{font-family:'微软雅黑'}.tc{text-align:center}.tr{text-align:right}.tl{text-align:left}.tdl{text-decoration:underline}.tdn,.tdn:hover,a.tdl:hover{text-decoration:none}.f0{font-size:0}.f10{font-size:10px}.f12{font-size:12px}.f13{font-size:13px}.f14{font-size:14px}.f16{font-size:16px}.f20{font-size:20px}.f24{font-size:24px}.vm{vertical-align:middle}.vtb{vertical-align:text-bottom}.vt{vertical-align:top}.vn{vertical-align:-2px}.vimg{margin-bottom:-3px}.pl10{padding-left:10px}.mr10{margin-right:10px}.cso{cursor:pointer}.pd10{padding:10px}.dn{display:none}*{box-sizing:border-box}body{background:#f8f8f8;font-size:16px}body .hide{display:none}body .form-group{margin-bottom:15px}body label{display:inline-block;max-width:100%;margin-bottom:5px;font-weight:700}body .form-control{display:block;width:100%;height:34px;padding:6px 12px;font-size:14px;line-height:1.42857143;color:#555;background-color:#fff;background-image:none;border:1px solid #ccc;border-radius:4px;box-shadow:inset 0 1px 1px rgba(0,0,0,0.075);transition:border-color ease-in-out .15s,box-shadow ease-in-out .15s}body .diaBox{width:100%;height:100%;box-shadow:0 0 7px #aaa;border:1px solid #aaa;border-radius:5px}body .diaBox .y-top{height:30px;line-height:30px;background:rgba(219,235,230,0.2)}body .diaBox .y_center{width:100%;box-sizing:border-box}body .diaBox .y_bottom .btn{display:inline-block;padding:6px 12px;margin-bottom:0;font-size:14px;font-weight:400;line-height:1;text-align:center;white-space:nowrap;vertical-align:middle;-ms-touch-action:manipulation;touch-action:manipulation;cursor:pointer;-webkit-user-select:none;-moz-user-select:none;-ms-user-select:none;user-select:none;background-image:none;border:1px solid transparent;border-radius:4px;margin-left:5px}body .diaBox .y_bottom .btn-defalut{color:#333;background-color:#fff;border-color:#ccc}body .diaBox .y_bottom .btn-primary{color:#fff;background-color:#337ab7;border-color:#2e6da4}body .diaBox .y_bottom .btnposi{position:absolute;bottom:10px;right:10px}
> js部分
 ```python
!function(t){t.fn.dialog=function(n){function i(){t(".diaBox").parent("div").remove(),e=0}var e=0,d={title:"标题",content:"",contentDom:"",btns:[{elId:"ok",els:"btn btn-primary",text:"确定",handler:function(){alert("确定"),i()}},{elId:"cancel",els:"btn btn-defalut",text:"取消",handler:function(){alert("取消"),i()}}],width:500,height:"auto"},s=t.extend(d,n),o=this;o.on("click",function(){if("0"==e){var n=document.createElement("div");t(n).css({width:s.width,height:s.height,position:"absolute",top:"10%",left:"30%"});var i=[];s.btns.length>0&&t.each(s.btns,function(t,n){i.push("<button id='"+s.btns[t].elId+"' class='"+s.btns[t].els+"'>"+s.btns[t].text+"</button>")}),i=i.join("");var d="";d=t(s.content.length>0?"<div class='diaBox'><div class='y-top'><div class='l pl10'>"+s.title+"</div><div class='r mr10 y_close cso'> × </div></div><div class='pd10 y_center'>"+s.content+"</div><div class='pd10 y_bottom'><div class='r btnposi'>"+i+"</div></div></div>":"<div class='diaBox'><div class='y-top'><div class='l pl10'>"+s.title+"</div><div class='r mr10 y_close cso'> × </div></div><div class='pd10 y_center'>"+t(s.contentDom).html()+"</div><div class='pd10 y_bottom'><div class='r btnposi'>"+i+"</div></div></div>"),d.appendTo(n),t(n).appendTo(t("body")),e=1}t(".y_close").on("click",function(){t(n).remove(),e=0})}),s.btns.length>0&&t.each(s.btns,function(n,i){t("body").on("click","#"+s.btns[n].elId,s.btns[n].handler)})}}(jQuery);
  ```



More info: [麦麦](maimai123.github.io)
