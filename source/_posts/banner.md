---
title: jquery 轮播图插件教程
date: 2017-06-09
tags: jq
---
简单的轮播插件，和一些大牛写的轮播是不能比的，一般简单的应用可以实现。支持淡入淡出和左右移动两种方式，支持自动轮播！
<font size=1>**如有转载，请注明出处。**</font>
><i class="icon-cloud"></i>简单的图片轮播
效果图：
<img src="https://github.com/maimai123/maimai123.github.io/blob/master/img/lbt.png?raw=true" />
 ```python
 ;(function($){
   $.fn.Carousel = function(options){
     var obj = $(this);
     var defaults = {
       'width' : "100%",
       'height' : "480",
       'speed' : "1000",
       'effect' : "fade",
       'delay' : "200",
       "doc" : true ,
       "arrow" : true
     };
     var opts = $.extend(defaults,options);
     // 设置容器的宽高
     obj.css({
       'width':opts.width,
       'height':opts.height
     });
     // 设置图片的宽高和容器一致
     obj.find("img").css({
       'width':opts.width,
       'height':opts.height
     });
     //轮播图片个数 4
     var length = obj.find('li').length;
     var str = "";
     obj.find(".wrapUl").css({"height":opts.height});
     obj.find(".warp").css({'width':opts.width * length});
     for(var i = 0;i<length;i++){
       str += "<li></li>";
     }
     // 如果显示小圆点
     if( opts.doc ){
       $("<ul class='docBox'>"+str+"</ul>").insertAfter(obj.find("div"));
       $(".docBox li:first").addClass("active");
     }
     // 小圆点情况下，如果效果为淡入淡出
     if( opts.effect == 'fade' ){
       obj.find('.warp li').css({
         "position":'absolute',
         "display":'none'
       }).first().show();
       var ii = 0;
       var setTime = setInterval(function(){
         obj.find('.warp li').stop(false,true).fadeOut(opts.delay).eq(ii).fadeIn(opts.delay);
         $(".docBox li").eq(ii).addClass("active").siblings().removeClass("active");
         ii++;
         if( ii == length ){
           ii = 0;
         }
       },opts.speed);
       $(".docBox li").on("click",function(){
         clearInterval(setTime);
         var index = $(this).index();
         setTimeout(function(){
           obj.find('.warp li').stop(false,true).fadeOut(opts.delay).eq(index).fadeIn(opts.delay);
         },opts.delay);
         $(this).addClass("active").siblings().removeClass("active");
       });
     }
     // 小圆点情况下，如果效果为平移
     if( opts.effect == 'move' ){
       var mm = 0;
       var setMove = setInterval(function(){
         obj.children('.warp').animate({"left":-(mm * opts.width)+"px"},opts.speed);
         $(".docBox li").eq(mm).addClass("active").siblings().removeClass("active");
         mm = mm+1;
         if( mm == length ){
           mm = 0;
         }
       },opts.speed);
       $(".docBox li").on("click",function(){
         clearInterval(setMove);
         var index = $(this).index();
         var left = index * opts.width;
         setTimeout(function(){
           obj.children('.warp').animate({"left":-left+"px"},opts.speed);
         },opts.delay);
         $(this).addClass("active").siblings().removeClass("active");
       });
     }
   };

 })(jQuery);

//调用
$(".banner").Carousel({
  'width' : "1200",  //设置容器宽度(暂不支持自适应)
  'height' : "700", //设置容器高度
  'effect' : "move",  //值为fade为淡入淡出效果 move则为左右移动
  'speed' : "1000",  //设置左右移动速度
  'delay' : "200"    //设置淡入淡出速度
});
  ```

More info: [麦麦](maimai123.github.io)
