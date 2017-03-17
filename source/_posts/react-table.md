---
title: 初学react 简单的table（包含增删查）-by 麦麦
---
><i class="icon-cloud"></i>react table
> 注意：react的script标签必须type为"text/babel"！！！
#主图
<img src="https://github.com/maimai123/maimai123.github.io/blob/master/img/react.png?raw=true" />
#点击新增
<img src="https://github.com/maimai123/maimai123.github.io/blob/master/img/new.png?raw=true" />
#搜索
<img src="https://github.com/maimai123/maimai123.github.io/blob/master/img/search.png?raw=true" />
部分代码如下：
 ```python
 var CartBox = React.createClass({
   //初始化数据，由state控制
   getInitialState:function(){
     return {
       Itemd :
       [
         {"goodsname":"手机","batch":"P1556","goodscolor":"金色","price":"￥5876"},
         {"goodsname":"杯垫","batch":"P1586","goodscolor":"红色","price":"￥56"},
         {"goodsname":"杯子","batch":"P1542","goodscolor":"红色","price":"￥76"},
         {"goodsname":"钢笔","batch":"P7656","goodscolor":"粉色","price":"￥66"},
         {"goodsname":"书本","batch":"P3556","goodscolor":"白色","price":"￥6"}
       ],
       selItem :[
         {"goodsname":"手机","batch":"P1556","goodscolor":"金色","price":"￥5876"},
         {"goodsname":"杯垫","batch":"P1586","goodscolor":"红色","price":"￥56"},
         {"goodsname":"杯子","batch":"P1542","goodscolor":"红色","price":"￥76"},
         {"goodsname":"钢笔","batch":"P7656","goodscolor":"粉色","price":"￥66"},
         {"goodsname":"书本","batch":"P3556","goodscolor":"白色","price":"￥6"}
       ]
     }
   },
   handleChange: function (rows) {
        this.setState({
            selItem : rows
        });
    },
   render : function(){
     return(
       <div>
         <WarpLeft onSear={this.handleChange} Itemdata={this.state.Itemd} />
         <Prop onAdd={this.handleChange} Itemdata={this.state.Itemd} />
         <WarpRight onDel={this.handleChange} Itemdata={this.state.selItem} />
       </div>
     );
   }
 });
 //搜索
 var WarpLeft = React.createClass({
   handleAdd :function(e){
     e.preventDefault();
     var self = this;
     var string = self.refs.findVal.value;
     var rows = self.props.Itemdata;
     var dataIn = "";
     var array =[];
     if(string.length > 0){
       $.each(rows,function(i,item){
         dataIn = item.goodsname.indexOf(string);
         if( dataIn > -1){
           array.push(item);
         }
       });
     }else{
       array = self.props.Itemdata;
     }
     this.props.onSear(array);
   },
   render :function(){
     var self = this;
     return(
       <div className="pull-left f2 he6 mt12">
         <form>
           <input id="search" ref="findVal" type="text" autoComplete="off" placeholder="请输入名称搜索"/>
           <button className="btn-sm r btn" onClick={self.handleAdd}>搜索</button>
         </form>
       </div>
     );
   }
 });
 //新增弹出框
 var Prop = React.createClass({
   handleAdd : function(e){
     var self = this;
     $(".propBox").show();
   },
   handleCan : function(e){
     var self = this;
     $(".propBox").hide();
   },
   handleYes : function(e){
     var self = this;
     var goodsname = self.refs.goodsname.value.trim();
     var batch = self.refs.batch.value.trim();
     var goodscolor = self.refs.goodscolor.value.trim();
     var prize = self.refs.prize.value.trim();
     var rows = self.props.Itemdata;
     if (goodsname != '' & batch != '' & goodscolor != '' & prize != '') {
        rows.push({'goodsname':goodsname,'batch':batch,'goodscolor':goodscolor,'price':prize});
        this.props.onAdd(rows);
     }
     self.refs.goodsname.value = "";
     self.refs.batch.value= "";
     self.refs.goodscolor.value="";
     self.refs.prize.value="";
     $(".propBox").hide();
   },
```
<!--more-->
```python
render :function(){
  var self = this;
  return (
    <div>
    <button onClick={self.handleAdd} className="addNew btn mt10 btn-primary">新增</button>
    <div className="propBox hide"> <div onClick={self.handleCan} className="close">×</div>
    <br/>
      <div className="row">
        <label htmlFor="goodsname">物品：</label><input ref="goodsname" type="text" id="goodsname"/>
      </div>
      <div className="row">
        <label htmlFor="batch">批号：</label><input ref="batch" type="text" id="batch"/>
      </div>
      <div className="row">
        <label htmlFor="goodscolor">颜色：</label><input ref="goodscolor" type="text" id="goodscolor"/>
      </div>
      <div className="row">
        <label htmlFor="prize">价格：</label><input ref="prize" type="text" id="prize"/>
      </div>
      <div className="row tc">
        <button onClick={self.handleYes} className="btn btn-sm btn-primary">确定</button>
      </div>
    </div>
    </div>
);
}
});
//展示数据
var WarpRight = React.createClass({
handleDel : function(e){
  var delIndex = e.target.getAttribute('data-key');
  this.props.Itemdata.splice(delIndex, 1);
  this.props.onDel(this.props.Itemdata);
},
render :function(){
  var self = this;
  return(
    <div className="pull-right f6 he6 l">
    <table className="table mt10">
      <thead>
        <tr>
          <th>物品</th>
          <th>批号</th>
          <th>颜色</th>
          <th>价格</th>
          <th>操作</th>
        </tr>
      </thead>
      <tbody>
      {
        this.props.Itemdata.map( function(item,i){
          return (
            <tr key={i}>
              <td key={i}>{item.goodsname}</td>
              <td>{item.batch}</td>
              <td>{item.goodscolor}</td>
              <td>{item.price}</td>
              <td><button onClick={self.handleDel} className="destroy"  data-key={i} >delete</button></td>
            </tr>
          )
        })
      }
      </tbody>
    </table>
    </div>
  );
}
});
ReactDOM.render(
<CartBox />,
document.getElementById("warp")
);
```

More info: [麦麦](maimai123.github.io)
