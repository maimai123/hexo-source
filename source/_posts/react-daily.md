---
title: react日常小难题整理
date: 2017-12-30
tags: React
---
初学react，在码代码过程中会遇到对大牛来说听起来愚蠢的问题。没错！这篇文章就是来整理这些愚蠢的问题的~ （嘿嘿嘿）下次可不要再丢人啦~
<font size=1>**如有转载，请注明出处。**</font>
## React
## 1、style问题在react中
> 大兄弟~ 你是否想在react中使用行内样式
> 如： style="height:30px"
> 然后你仰望星空 然后你开始怀疑自己，难道是我写错了？哈哈 其实是在react中的写法不太一样而已~~~
> 看！写成这样就没错列~
<img src="https://github.com/maimai123/maimai123.github.io/blob/master/img/style.png?raw=true" />

## 2、从后台拿到数组遍历成字符串类型的标签，如何被浏览器识别
> 在用antd表格的时候，我曾遇到过这个问题。后台传过来的是数组，遍历成n个带url的a标签 显示在表格的操作栏
> 一开始我先将数组遍历加入字符串a标签放入一个新数组 最后将此数组转成字符串 return出去变成了字符串 没有被浏览器识别 看下图
<img src="https://github.com/maimai123/maimai123.github.io/blob/master/img/return.png?raw=true" />
> 后来找到了解决方法 如下 (ps:千万不要忘了给数组加上key，否则控制台会提示哦)
```javascript
return files.map((file,index)=>{
  return <a key={index} href=""><Icon type="paper-clip" /> {file} </a>
})
```

<!--more-->
## 3、value 和 defaultValue 表格中回显问题---组件使用的是[ant design](https://ant.design/)
> 对于表格回显问题 拿到数据以后（暂且用暂存state的方法）显示到表单中，如果用defaultValue的话，只在第一次有效，点击其他详情出现的永远是第一个详情。
> 如果用value的话，回显正常，但当要修改的时候是不能修改的。因为value是受限的。
> 目前的解决方法，每次关闭model以后都销毁里面的form，再次打开的时候新引一遍model。传值的话用props传，再用defaultValue就没有问题了。做法如下：
>新建一个form的组件，如
```javascript
 const FromModel =React.createClass({
  render(){
    var self = this;
    const formItemLayout = {
      labelCol: { span: 6 },
      wrapperCol: { span: 14 },
    };
    return(
      <Form horizontal>
        <Row gutter={24}>
          <Col sm={12}>
            <FormItem
              label="人才姓名"
              {...formItemLayout}
            >
              <Input size="default" defaultValue={this.props.record.name} />
            </FormItem>
          </Col>
        </Row>
      </Form>
    )
  }
})
引用model的情况如下，亲测没有任何问题：
<Modal title="修改" visible={this.state.visible}
       onOk={this.handleOk} onCancel={this.handleCancel}
 >
   {
      self.state.visible ?  //判断visible为true还是false false的话设为null
      <FromModel ref="modelform" record={self.state.record}/>
       :null
    }
</Modal>
```

## 4、url地址栏中一直有/#/xxx的问题
> 这是一个你会获取到的默认 history ，如果你不指定某个 history 。
> 它用到的是 URL 中的 hash（#）部分去创建形如 example.com/#/some/path 的路由。这个 支持 ie8＋ 的浏览器，但是因为是 hash 值，所以不推荐使用。
> browserHistory 是由 React Router 创建浏览器应用推荐的 history。
> 它使用 History API 在浏览器中被创建用于处理 URL，新建一个像这样真实的 URL example.com/some/path。
> 使用 **browserHistory** 代替 **hashHistory**就可以解决这个问题啦~

## 5、es5和es6写法对比
<img src="https://github.com/maimai123/maimai123.github.io/blob/master/img/es6.png?raw=true" />

## 6、jsx语法中不允许判断的问题
> 在jsx语法内部是不允许if判断的，即如下代码是会报错的：
<img src="https://github.com/maimai123/maimai123.github.io/blob/master/img/jsx.png?raw=true" />
> 对于判断条件简单的用三目运算符 如：
```javascript
React.render(
  return(
      <div id={condition ? 'true的情况' : 'false的情况'}>
          Hello World!
      </div>
    )
, mountNode);
```
> 对于判断条件复杂的用变量 如：
<img src="https://github.com/maimai123/maimai123.github.io/blob/master/img/jsx2.png?raw=true" />

## 7、父组件与子组件之间的传值问题
> 对于不会redux的童鞋，请乖乖的用props来传值吧~ 具体教程参考[阮一峰---React 入门实例教程](http://www.ruanyifeng.com/blog/2015/03/react.html)

## 8、关于在react中想要使用jquery操作dom的问题
> 虽然react很好用是吧，但在jquery中一行代码实现切换class的工作量在react中却要考虑很多，所以我就比较喜欢引入jquery解决这种问题啦~
> 直接用$(this)并不能获取到你想要的东西，你必须在onclick={(ev)=>self.doSomething(ev)}中将事件传递过去，然后在方法中你就可以使用$(ev.target)获取到啦 :-D

## 9、数组嵌套深层拷贝问题
> 近期遇到的一个头疼的问题，后台返回数据（数组嵌套对象再嵌套数组）一次性给我，我得去处理第一次显示一部分，
> 第二次才显示完整，用到了concat()拷贝深度数组并不起效果，
> 最后。。。终于给我找出来了 记录一下
```javascript

function clone(source)
{
  var result;
  (source instanceof Array) ? (result = []) : (result = {});

  for (var key in source) {
  result[key] = (typeof source[key]==='object') ? clone(source[key]) : source[key];
  }
  return result;
}

```
## 10、对于在react中字符拼接的问题
> 不知道大家发现没有，在react中在遍历中想要字符拼接变量用以下的方法是行不通的
```javascript
 <a href="xxxxx.html?code="+usercode>我要传参数跳转的啦~</a>
```
> 我之前一直深受其扰，只能先定义一个变量，将跳转链接放入变量再拼接方可成功，后来查阅得出
> 改为
```javascript
 <a href={"xxxxx.html?code="+usercode}>我要传参数跳转的啦~</a>
```

## 11、ant design中的图片轮播报错问题
```javascript
let imgTpl = (IMAGE_DATA || []).map((item, index) => {
   return (
   <div key={index} >
      <a href={item.bannerUrl == '' ? "javascript:;" : item.bannerUrl}><img src={item.url} alt={item.bannerName}/></a>
   </div>
   );
});
if (!IMAGE_DATA.length) imgTpl = <div></div>;
```

## 12、请耐心等待，麦麦会不定期更新哒~

More info: [麦麦](https://github.com/maimai123)
