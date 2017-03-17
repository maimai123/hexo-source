---
title: 日常小计（麦麦）
---
<h1>无图片时显示默认图片</h1>
<font size=1>**如有转载，请注明出处。**</font>
##第一种
>$("img“).on('error',function(){
    this.src = '/assets/img/student/null-pic.jpg';  //默认图片
});

##第二种
>imgShow:function(){
    var t_img; // 定时器
    var isLoad = true; // 控制变量
    // imgBlock();
    imgReplace();
    function imgReplace(){
        jquery("img").each(function(){
            jquery(this).on('error',function(){
                jquery(this).attr('src','/assets/img/student/null-pic.jpg')
            })
        })
    }
    function imgBlock(callback){
        $('img').each(function() {
            if (!this.complete || typeof this.naturalWidth == "undefined" || this.naturalWidth == 0)  {
                this.src = '/assets/img/student/null-pic.jpg';
                isLoad = false;
                return false;
            }
        });
        if(isLoad){
            clearTimeout(t_img); // 清除定时器
            // 回调函数
            callback;
            // 为false，因为找到了没有加载完成的图，将调用定时器递归
        }else{
            isLoad = true;
            t_img = setTimeout(function(){
                imgBlock(callback); // 递归扫描
            },100); // 我这里设置的是500毫秒就扫描一次，可以自己调整
        }
    }
}




More info: [麦麦](maimai123.github.io)
