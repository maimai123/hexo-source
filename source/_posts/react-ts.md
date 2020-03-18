---
title: react+ts初体验
date: 2020-03-18
tags: react
---
哈哈哈 终于对react+ts下手了，其实本身也是挺反感那么严重的规范性的。但是想想这样子虽然时间花的久一点，但是好处多多啊。最后还是接受了ts！！！（小声BB：其实是因为公司要用没办法才接受的）
anyway！！我们下来来学学看！

# let's start!
<font size=1>**如有转载，请注明出处。**</font>
> 在react中创建组件的形式有三种

> 纯函数式定义的无状态组件
> React.createClass 定义的组件（不做解释）
> Extends React.Component 定义的组件

纯函数式定义的无状态组件及类组件的到底有什么不同呢？

```javascript
  // React.Component是以ES6的形式来创建react的组件的，是React目前极为推荐的创建有状态组件的方式，最终会取代React.createClass形式

  export interface IProps { // 可以导出interface 以供其他地方也可以使用
    onChange: Function;
  }

  interface IState {
    visible: boolean;
  }

  export default class Example extends Component<IProps, IState> {
    static defaultProps = {
      onChange: () => {}
    };

    constructor(props: IProps) {
      super(props);
      this.state = {
        visible: true,
      };
    }

    handleChange () { // 推荐所有方法名都以handle或者on开头
      const { visible } = this.state;
      this.setState({
        visible: !visible
      });
    }

    render() {
      const { visible } = this.state;
      return (
        <div className={styles.example}>
          { visible ? '这里是React.Component创建组件的demo呀！' : '现在看不见了吧~' }
          <button onClick={this.handleChange}>开关</button>
        </div>
      );
    }
  }
```
> 纯函数组件的特点:

> 1、组件不会被实例化,整体渲染性能得到提升
> 2、组件不能访问this对象
> 3、组件无法访问生命周期的方法 （可以借助hooks拿到哦）
> 4、无状态组件只能访问输入的props,无副作用

```javascript
// 简单的，一般在页面内部会定义一些
  function DemoComponent(props) {
    return <div>Hello {props.name}</div>
  }
// 稍复杂的
  interface IProps {
    name: string;
  }

  const Example: React.FC<IProps> = props => {
    const [visible, setVisible] = useState(true);
    const { name } = props;


    useEffect (
      () => {
        setVisible(true)
      },
      [] // 空数组代表只渲染一次，同componentDidMount，有兴趣的朋友可以去看下react Hooks哦 我之前的文章里也有写
    )

    handleToggle () { // 尽量不要使用箭头函数，会导致某些参数获取不到
      setVisible(!visible)
    }

    return (
      <div className={styles.example}>
        { name }
        { visible ? '这里是React.Component创建组件的demo呀！' : '现在看不见了吧~' }
        <button onClick={handleToggle}>开关</button>
      </div>
    )
  };
```

> react中使用富文本
我用的是[wangEditor](http://www.wangeditor.com/)

```javascript
  import Editor from 'wangeditor';

  componentDidMount() { // 先注册一个编辑器
    const { content } = this.state;
    const elem = this.editorElem;
    this.editor = new Editor(elem);
    this.editor.customConfig.menus = [ // 设置编辑器上面的配置项
      'head',
      'bold',
      'fontSize',
      'foreColor',
    ];
    this.editor.customConfig.onchange = (html: any) => {
      this.setState({
        content: html
      });
    }
    this.editor.create();
  }

  // 在需要填入数据的地方用一下方法就可以实现回填拉 亲测好用哦
  this.editor.txt.html(content);

  <div ref={ref => { this.editorElem = ref }}></div>
```

More info: [麦麦](maimai123.github.io)
