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

## 请耐心等待，麦麦会不定期更新哒~
More info: [麦麦](https://github.com/maimai123)
