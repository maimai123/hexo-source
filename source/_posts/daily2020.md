---
title: React中父组件通过ref调用子组件方法时报错
date: 2020-02-26
tags: React
---
<font size=1>**转载至[风吹过的夏夜](https://segmentfault.com/a/1190000015172005?utm_source=channel-hottest)**</font>

在项目中用了dva，导致了在用Ref方式取子组件的方法时，用常用的方法报错
寻寻觅觅找到了解决方法

接下来我会用一个简单的demo来演示这个问题是如何发生以及如何解决的：

```javascript
// 这里是子组件
class Child extends Component {
    constructor(props) {
        super(props);
        this.state = {
            value: 0
        }
    }

    handleAdd = () => {
        this.setState({
            value: this.state.value + 1
        })
    };

    render() {
        return (
            <div>
                <p>{this.state.value}</p>
            </div>
        )
    }
}

export default Child

// 这里是父组件
class Father extends Component {
    clickHandler = () => {
        this.child.addHandler();
    };

    render() {
        return (
            <Provider store={store}>
                <div className="App">
                    <Child ref={ref => this.child = ref} />
                    <button onClick={this.clickHandler}>+1</button>
                </div>
            </Provider>
        );
    }
}

export default Father;

```
这样是没有问题了，但当子组件需要在Redux/dva（dva基于 redux 和 redux-saga）中读取的一个数据，那么就需要connect一下了

```javascript
export default connect(mapStateToProps, mapDispatchToProps)(Child)
```

嗯，看起来很完美，点击，然后....boom( ⊙ o ⊙ )报错了！

不科学啊，说到这里相信大家都能猜出来事connect过后导致的问题，所以去官方文档翻箱倒柜一下connect([mapStateToProps], [mapDispatchToProps], [mergeProps], [options])，
果不其然，原来connect参数有4个，所以可能是遗漏了某个参数吧，所以继续翻...
然后看到这样一段话：
> [withRef] (Boolean): If true, stores a ref to the wrapped component instance and makes it available via getWrappedInstance() method. Default value: false

> 简单理解下，就是说connect过后export出去的不是组件本身，而是经过包装处理的组件，官方称之为wrapped component，所以默认是不将ref存储到这个包装对象里的(Default value: false)，因此只有将withRef这个参数置为true，那么Redux就会将ref存储到这个包装对象里以供使用了，并且请注意下via getWrappedInstance() method这段话，即便我们将withRef置为true但没有通过getWrappedInstance()获得原对象的ref(reference)也是不行的。所以说道这里大家也知道解决方案是什么了：

> 1）connect导出Addition组件时候添加withRef参数：
```javascript
export default connect(mapStateToProps, mapDispatchToProps, null, {withRef: true})(Addition)
```
> 2）调用addHandler方法前使用getWrappedInstance()获得原对象的ref：
```javascript
this.refs.addition.getWrappedInstance().addHandler()
```

啦啦啦~ 这样就好啦

More info: [麦麦](maimai123.github.io)
