---
title: jquery 简单插件教程（麦麦）
---
插件的引入使我们的代码变得越来越简单，我们只用过插件却很少有人动手写过插件。本文就以实例形式简单叙述了jQuery插件的实现方法。分享给大家供大家参考之用。
<font size=1>**如有转载，请注明出处。**</font>
><i class="icon-cloud"></i>简单的alert
 ```python
//弹出alert弹框
$.fn.alert = function(options){
  var dets = {
    Event : "click",  //默认使用click事件
    msg : "hello world!" //默认弹出内容为hello，world
  }
  var opts = $.extend(dets,options);
  var $this = $(this);
  $this.on( opts.Event ,function(e){
    alert(opts.msg);
  });
}

//调用alert插件
$("div").alert({
  Event :　"mouseover",
  msg : "我改变了默认弹出内容"
});
```
<!--more-->
><i class="icon-cloud"></i>输入自动完成插件
<img src="https://github.com/maimai123/maimai123.github.io/blob/master/img/image.png?raw=true" />
 ```python
 ;(function($){
   $.fn.autocomplete = function(options){
     var defaults ={
       borderColor : "#C7F4EE"
     };
     var opts = $.extend(defaults,options);
     var self = $(this);
     flag = '0'; //没有下拉框
     record ="";
     // 发起ajax请求拿数据
     $.ajax({
       url : '/autocomplete/json.htm',
       type : 'GET',
       dataType : 'json',
       success : function(data){
         record = data; //取到json数据
       }
     });
     function getFocus(){
       var len = self.val().length;
       var str = self.val().toLowerCase();
       if(len > 0){ //输入框中有数据
           if(flag == '0'){
             $("<div class='down'></div>").css({
                 'width':self.outerWidth()-2,
                 'border': '1px solid',
                 'border-color':opts.borderColor,
                 'border-top':'none',
                 'overflow' : 'hidden'
               }).insertAfter(self);
               flag = '1'; //容器已显示
               // 遍历 data 取数据放入容器
               $.each( record ,function(i,v){
                 $.each(v,function(j,k){
                 var jsonText = k.text;
                 var jsonConvert = k.convert;
                 if( jsonText.indexOf(str) > '-1'){
                   if(jsonConvert){
                     $("<div class='single'>"+jsonConvert+"</div>").appendTo($(".down"));
                   }else{
                     $("<div class='single'>"+jsonText+"</div>").appendTo($(".down"));
                   }
                 }
                 });
               });

           }
       }else{ //输入框中无数据
         $(".down").remove();
         flag = '0';
       }
     }
     self.on({
       keyup :function(e){
         $(".down").remove();
         flag = '0';
         getFocus();
         function translate(){
           var string =self.val().toLowerCase();
           $.each( record ,function(i,v){
             $.each(v,function(j,k){
             var jsonText = k.text;
             var jsonMean = k.mean;
             if( jsonText.indexOf(string) > '-1'){
               $("#show").text(string+" ------ " + jsonMean);
             }
             });
           });
         }
         $(".down").on('click',".single",function(){
           self.val($(this).text());
           translate();
           $(".down").remove();
           flag = '0';
         });
         self.siblings('button').on('click',function(){
           translate();
         });
       },
       blur : function(e){
         setTimeout(function(){
           $(".down").remove();
           flag = '0';
         },200);
       }
     });

   };
 })(jQuery);
//调用
$("#myInput").autocomplete();
  ```



More info: [麦麦](maimai123.github.io)
