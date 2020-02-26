---
title: 日常小计~
date: 2018-04-28
tags: 日常
---
讲的都是些很基础的东西，觉得以后写代码能直接复制粘贴的哈哈哈~
<font size=1>**如有转载，请注明出处。**</font>
## Intersting
## 1、移动端搜索框（键盘中是点击前往按钮）
```javascript
<form action="" @keydown="handleKeydown">
  <input
    ref="$input"
    type="text"
    v-model="val"
    @focus="isShow = true"
    class="search-input"
    placeholder="搜索">
    <span v-show="isShow" @click="handleBlur">取消</span>
</form>
```
* 其实就是在input框外面加个form，哈哈哈~ 如果有其他更好的方法要跟我说哦~

## 2、split按照不同分隔符进行分割
```javascript
'asdas,sdad，asdasd'.split(/[,，]/);
```
* 在前台展示是发现后台可能会传中英文的逗号过来，我就用正则进行分割啦哈哈

<!--more-->
## 3、如果你想计算数组中元素出现的次数或者想把数组转为对象，那么你可以使用 reduce 来做到。
```javascript
const cars = ['BMW','Benz', 'Benz', 'Tesla', 'BMW', 'Toyota'];
const carsObj = cars.reduce((obj, name) => {
  obj[name] = obj[name] ? ++obj[name] : 1;
    return obj;
  }, {});
  carsObj; // => { BMW: 2, Benz: 2, Tesla: 1, Toyota: 1 }
}
```

## 4、 mapActions别名
```javascript
...mapActions('option/product', ['FETCH', 'CREATE', 'EDIT', 'DEL']),

...mapActions({
  foo: 'some/nested/module/foo',
  bar: 'some/nested/module/bar'
})
```
## 5、Cube-ui slide引用不到的问题
* SlideItem在slide的Item中
* cubeSlideItem: Slide.Item,

## 6、element-ui中验证对象中的属性
* prop这样写： prop="commercialInsurance.renewalDate"

```javascript
// 验证规则这样写：
rules: {
  'commercialInsurance.renewalDate': [
  { required: true, ...}
  ],
}
```

## 7、请耐心等待，麦麦会不定期更新哒~
More info: [麦麦](https://github.com/maimai123)
