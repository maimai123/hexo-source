---
title: 面试题整理 - 高频手写面试题
date: 2022-01-18
tags: interview
---
面试题整理
<font size=1>**如有转载，请注明出处。**</font>

## 手写 instanceof
先了解instanceof是用来干啥的？一般讲到instanceof都会先想到typeof
> typeof 与 instanceof 都是判断数据类型的方法，typeof会返回一个变量的基本类型，对于引用类型除了function以外，其他都无法判断，都返回object，而instanceof无法判断基础类型， 可以判断复杂引用数据类型。
instanceof作用:
> 判断一个实例是否是其父类或者祖先类型的实例。
instanceof 在查找的过程中会遍历左边变量的原型链，直到找到右边变量的 prototype查找失败，返回 false
那么我们自己如何实现呢？
```javascript
// typeof 11 => number
// typeof [] => object
// typeof function => function

// 11 instanceof Number => false
// [] instanceof Array => true

let myInstanceof = (target,origin) => {
    const baseType = ['string', 'number','boolean','undefined','symbol']
    if (baseType.includes(typeof(target))) { return false }
    while(target) {
      if (target.__proto__ === origin.prototype) {
        return true
      }
      target = target.__proto__;
    }
    return false
}
```
##实现数组的map方法
> 数组的map() 方法会返回一个新的数组，这个新数组中的每个元素对应原数组中的对应位置元素调用一次提供的函数后的返回值, 如果是判断运算，不满足条件返回false，满足返回true。且map是个纯函数，即不会改变原有数组。

```javascript
Array.prototype.newMap = function(fn) {
   const newArr = [];
　　for (let i = 0; i < this.length; i++) {
　　　　newArr.push(fn(this[i], i, this)) // 数组当前值，索引，数组对象
　　}
　　return newArr;
}
```
## 数组扁平化
把多维数组转成一维数组
```javascript
// 1、es6提供的新方法 flat(depth) ps:flat()默认只会“拉平”一层,如果想要“拉平”多层的嵌套数组,可以将flat()方法的参数写成一个整数,表示想要拉平的层数,默认为1。如果不管有多少层嵌套,都要转成一维数组,可以用Infinity关键字作为参数
let arr = [1,[2,3]];
arr.flat(1) // [1,2,3]
arr.flat(Infinity) // [1,2,3]
// 2、循环递归
function flatten(arr) {
  const res = [];
  for (let i = 0, length = arr.length; i < length; i++) {
  if (Array.isArray(arr[i])) {
    res.push(...flatten(arr[i])); //或者用扩展运算符
  } else {
      res.push(arr[i]);
    }
  }
  return res;
}
```
## 实现深拷贝
浅拷贝和深拷贝的区别：
浅拷贝：只拷贝一层，更深层的对象级别的只拷贝引用(指针指向同一个地址)
深拷贝：拷贝多层，每一级别的数据都会拷贝。这样更改拷贝值就不影响另外的对象

```javascript
// 原理是循环递归，为啥要用for in 呢，因为for in 会遍历原型上的方法。
const deepCopy = (obj) => {
  if(typeof obj !== 'object') return;
  let newObj = obj instanceof Array ? [] : {};
  for (let key in obj) {
    newObj[key] = typeof obj[key] === 'object' ? deepCopy(obj[key]) : obj[key];
  }
  return newObj;
};
// 还有人说可以使用 JSON.parse(JSON.stringify(Obj))，这种方式对正则表达式类型、函数类型等无法进行深拷贝(而且会直接丢失相应的值)。
```

## 手动实现new
new做了什么？
1、创建一个空对象 obj;
2、将空对象的隐式原型（__proto__）指向构造函数的prototype。
3、绑定this。（构造函数中的this指向新对象并且调用构造函数）
4、返回新对象。

```javascript
function create(){
 let obj={}
 //获取构造函数
 let Con=[].shift.call(arguments);
 obj.__proto__ = Con.prototype
 let result = Con.apply(obj,arguments);
 return typeof result === 'object' ? result : obj
}
```

## 手写promise
首先了解promise有三种状态：pending，fulfilled，rejected
call，apply,bind相同点和区别？
1、相同点
  这三者都是用来改变函数的this对象的指向的
  1、都是用来改变函数的this对象的指向的。
  2、第一个参数都是this要指向的对象。
  3、都可以利用后续参数传参。
2、区别
call和apply的第一个参数都是要改变上下文的对象，而call从第二个参数开始以参数列表的形式展现，apply则是把除了改变上下文对象的参数放在一个数组里面作为它的第二个参数。
call和apply改变了函数的this上下文后便执行该函数,而bind则是返回改变了上下文后的一个函数。bind方法是事先把fn的this改变为我们要想要的结果，并且把对应的参数值准备好，以后要用到了，直接的执行即可，也就是说bind同样可以改变this的指向，但和apply、call不同就是不会马上的执行

Array.prototype.slice.call(arguments)可以将arguments转换成数组

```javascript
const STATUS = {
 PENDING: 'pending',
 FULFILLED: 'fulfilled',
 REJECTED: 'rejected'
}
let i = 1;
// 构造函数Promise
function Promise(fn) {
    this.id = i++;

    this.status = STATUS.PENDING; // 初始化状态
    this.value = null;// 初始化值
    this.deffered = []; // 下一个执行的Promise是谁，子Promise

    // 3. 构造函数内调用函数（apply参数是数组，call参数是一个一个的，调用函数改变this的指向）
    // resolve和reject的this都是当前的Promise对象。 使用call立即执行，而使用bind方法不会立即执行函数，而是返回一个新的函数
    fn.call(this, this.resolve.bind(this), this.reject.bind(this) );
}

Promise.prototype = {
    constructor: Promise,
    resolve: function (data) {
        this.status = STATUS.FULFILLED;
        this.value = data;
        //执行后续行为
        this.done()
    },
    reject: function (err) {
        this.status = STATUS.REJECTED;
        this.value = err;
        //执行后续行为
        this.done()
    },
    done: function () {
        // 让这些this.deffered(子Promise执行)
        this.deffered.forEach(task => this.handler(task));
    },
    handler: function (task) {
        // 判断当前的执行的状态是咋样，调用对应的函数
        var status = this.status;
        var value = this.value;
        var p ;
        switch (status) {
            case STATUS.FULFILLED:
                p = task.onfulfilled(value)
                break;
            case STATUS.REJECTED:
                p = task.onrejected(value)
                break;
        }

        // 如果 p 是一个Promise的话，我们需要让他继续执行
        // 把后续（task.promise)的deffer交给这个p
        if(p && p.constructor === Promise){
            // 是下一个promise
            // 把下一个作为then链接的deffer移交p的deffered
            p.deffered = task.promise.deffered;
        }
    },
    then: function (onfulfilled, onrejected) {
        // 保存该函数
        var obj = {
            onfulfilled: onfulfilled,
            onrejected: onrejected
        }

        // 新来一个Promise 对象，让其存储这些
        // 并且能根据不同的Promise去then
        obj.promise = new this.constructor(function () {});

        console.log(this);  // 1
        console.log(obj.promise); // 2

        //保存接下来的子Promise
        //建立一个与下一个Promise之间的关系
        if(this.status == STATUS.PENDING) this.deffered.push(obj)

        // 保证不报错，未来不能return自己，需要换人
        return obj.promise;
    }
}
```

## 手写ajax

1、创建XMLHttpRequest对象
2、设置回调函数
3、使用open方法与服务器建立连接
4、向服务器发送数据

```javascript
function ajax(url, method="GET", data = null, async = true) {
  // 创建XMLHttqRequest
  const XHR = new XMLHttpRequest()
  // 设置请求状态改变时执行的函数
  XHR.onreadystatechange = function() {
    if (XHR.readyState === 4 && (XHR.status >= 200 && XHR.status < 300)) {
      var res = xhr.responseText;
      console.log(JSON.parse(res));
    } //XHR.responseText为响应体
  }
  // 初始化请求参数
  XHR.open(method, url, async)
  // 发起请求
  XHR.send(data)
}
```
## 手写防抖函数
连续不断触发时不调用，触发完后过一段时间调用

```javascript
function debounce (fn, delay) {
  if (typeof fn !== 'function') throw new TypeError('fn不是函数')
  if (timer) clearTimeout(timer)
  var _this = this;
  var args = arguments;
  timer = setTimeout(function () {
    fn.apply(_this, args);
  }, delay);
}
```

## 手写节流函数
固定函数执行的速率

```javascript
function throttle (fn, delay) {
  let timer;
  return function () {
    var _this = this;
    var args = arguments;
    if (timer) return;
    timer = setTimeout(function () {
      fn.apply(_this, args);
      timer = null;
    }, delay)
  }
}
```



## 请耐心等待，麦麦会不定期更新哒~
More info: [麦麦](https://github.com/maimai123)
