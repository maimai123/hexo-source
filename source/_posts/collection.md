---
title: 整理各对象常用属性与方法
date: 2017-04-05
tags: js
---
<font size=1>**如有转载，请注明出处。**</font>
------

**String对象的各种属性与方法**

属性：

>  i、length 字符串长度
   "aaa".length -> 3
>  ii、prototype允许您向对象添加属性和方法
 ```python
   例：
      String.prototype.spacify=function(str){
        return(str.split("").join(" "))
      }
      "asasda".spacify("asss") -> "a s s s" 新建的String对象的方法,用于往字符串中插入空格

```
方法：

> 1、split 讲字符分割成数组显示
      "a,s,d".split(",") -> ["a","s","d"]
> 2、concat 合并字符串火数组
     "1".concat("sdf","555")-> "1sdf555"
      ["1"].concat("sdf","555") ->["1", "sdf", "555"]
> 3、toLocaleUpperCase同toUpperCase 小写转换成大写
     "asaddas".toLocaleUpperCase()=="asaddas".toUpperCase() "ASADDAS"
> 4、toLocaleLowerCase同toLowerCase 大写转换成小写
     "ASADDAS".toLocaleLowerCase()=="asaddas".toLowerCase() "asaddas"
> 5、charAt 从字符串中取出单个字符
     "asde".charAt(2)-> d
> 6、indexOf("xx",[从几开始检索]) 从字符串中检索字符是否存在,不存在返回-1,
     "woshiyuyafei".indexOf("yuyafei") ->5
> 7、lastIndexOf("xx",[从几开始检索]) 从最后往字符串中检索字符是否存在,不存在返回-1,
     "woshiyuyafei".lastIndexOf("yu",11) ->5
> 8、match("xxx",'regexp') 检索字符串中指定值返回指定值，或找到一个或多个正则表达式的匹配,
     "hello world".match("hello") -> ["hello"]
     "he2llo1wo3rld".match(/\d+/g) -> ["2", "1", "3"]
> 9、slice(start,end) 提取字符串的某个部分，并以新的字符串返回被提取的部分(不包括结束),不申明结束则默认为字符串长度-1.若为负数，则变为长度+负数的值取 返回新的字符串
     "hello aaaa".slice(1,6) -> "ello "
    "hello aaaa".slice(-8,-2) -> "hello aaaa".slice(2,8) -> "llo aa"
> 10、substr(start,length) 从字符串中抽取从 start 下标开始的指定数目的字符,若为负数：-1 指字符串中最后一个字符，-2 指倒数第二个字符，以此类推。  返回新字符串
     "hello world".substr(1,4) -> "ello"
     "hello world".substr(-8,4)-> "hello world".substr(3,4)  -> "lo w"
> 11、substring(start,end) 提取字符串中介于两个指定下标之间的字符(不包括结束不接受负数)
     "hello".substring(1,3) -> "el"

<!--more-->
##Number对象的各种属性与方法

方法：

> 1、toString([几进制显示数字]) 把数字转换成字符串
      123.toString() -> 报错：这是因为javascript引擎在解释代码时对于“1.toString()”认为“.”是浮点符号，但因小数点后面的字符是非法的，所以报语法错误；
    而后面的“1..toString()和1.2.toStirng()”写法，javascript引擎认为第一个“.”小数点，的二个为属性访问语法，所以都能正确解释执行；
    对于“(1).toStirng()”的写法，用“()”排除了“.”被视为小数点的语法解释，所以这种写法能够被解释执行；
       var num = 111; num.toString();  -> "111" 或 (123).toString()
       {}.toString() 报错 表達式語句不能以 { 或 function 開頭 改成({}).toString() 输出 "[object Object]"
> 2、toFixed() 把数字四舍五入为指定小数位数的数字
      var num = 1223; num.toFixed(2); -> "1223.00"
> 3、toPrecision( 1 ~ 21 之间的整数) 在对象的值超出指定位数时将其转换为指数计数法
      var num =10000; num.toPrecision(4) -> "1.000e+4"


##Boolean对象的各种属性与方法

方法：

> 1、toSource() 返回表示对象源代码的字符串
     function employee(name,job,born)
     {
       this.name=name;
       this.job=job;
       this.born=born;
    }
     var bill=new employee("Bill Gates","Engineer",1985);
     bill.toSource();  ->({name:"Bill Gates", job:"Engineer", born:1985})
> 2、toString() 同number与String
> 3、valueOf() 返回 Boolean 对象的原始值
      var boo = new Boolean(false)
      document.write(boo.valueOf())  ->false

##Array对象的各种属性与方法
属性：
> 1、length 返回数组的长度
      ["1","2"].length  -> 2
方法：
> 1、concat() 合并数组
     ["1","2"].concat("123",[2]) -> ["1", "2", "123", 2]
> 2、join([分割的符号]) 把数组的所有元素放入一个字符串。元素通过指定的分隔符进行分隔。
     ["1","2"].join("|")  -> "1|2"
> 3、pop() 删除数组最后一个元素并返回杯删除的元素
     ["s","e"].pop()  -> "e"
> 4、push() 往数组中添加一个或更多元素，并返回新的长度。
     ["s","e"].push("32","45") -> 4
> 5、reverse() 颠倒顺序
     [2,5,3,4,1].reverse()  -> [1, 4, 3, 5, 2]
> 6、sort() 排序（但对数字排序不准确）
     [2,5,3,4,21].sort() -> [2, 21, 3, 4, 5]
> 7、shift() 把数组的第一个元素从其中删除，并返回第一个元素的值。
     [2,5,3,4,21].shift()  -> 2
> 8、slice(start,end) 从已有的数组中返回选定的元素,返回新数组
    [2,5,3,4,21].slice(1,4) -> [5, 3, 4]
> 9、splice(规定添加/删除项目的位置,要删除的项目数量,向数组添加的新项目) 向/从数组中添加/删除项目，返回被删除的项目。
     [2,5,3,4,21].splice(1,3,8888) -> [5, 3, 4]
> 10、toString() 把数组转换成字符串
     [2,5,3,4,21].toString() -> "2,5,3,4,21"
> 11、unshift() 向数组的开头添加一个或更多元素，并返回新的长度
     [2,5,3,4,21].unshift(1) -> 6

More info: [麦麦](maimai123.github.io)
