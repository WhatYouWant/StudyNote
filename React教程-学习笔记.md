#React教程-学习笔记

##React State(状态)
React 把组件看成是一个状态机（State Machines）。通过与用户的交互，实现不同状态，然后渲染 UI，让用户界面和数据保持一致。
React 里，只需更新组件的 state，然后根据新的 state 重新渲染用户界面（不要操作 DOM）。

##React Props
state 和 props 主要的区别在于 props 是不可变的，而 state 可以根据与用户交互来改变。这就是为什么有些容器组件需要定义 state 来更新和修改数据。 而子组件只能通过 props 来传递数据。

##React 组件 API
React组件API

名称 | 方法
------- | -------
设置状态 | setState
替换状态 | replaceState
设置属性 | setProps
替换属性 | replaceProps
强制更新 | forceUpdate
获取DOM节点 | findDOMNode
判断组件挂载状态 | isMounted

#例

```
<div id="example07"></div>
    <script type="text/babel"> //babel是JS与JSX转换编译器
      var MySelect = React.createClass({ //创建class
			
			//只在挂载前加载一次，返回的内容实际为为state创建的属性，可以通过 this.state.selected获取selected的属性值，初始化为false
        getInitialState: function(){
          return {selected: false};
        },
			
			//该属性为一个函数方法select
        select: function(event){
          console.log('textContent' + event.target.textContent);
          if (event.target.textContent === this.state.selected) {
            this.setState({selected: false});
          } else{
            this.setState({selected: event.target.textContent});
          }
        },

        render: function() {
          var mySelectStyle = {
            border: '1px solid #999',
            display: 'inline-block',
            padding: '5px'
          };

          return (
            <div style={mySelectStyle}>
              <MyOption state={this.state.selected} select={this.select} value="Volvo"></MyOption> //state属性值为this.state.selected的值， select属性为this.select这个回调方法
              <MyOption state={this.state.selected} select={this.select} value="Saab"></MyOption>
              <MyOption state={this.state.selected} select={this.select} value="Mercedes"></MyOption>
              <MyOption state={this.state.selected} select={this.select} value="Audi"></MyOption>
            </div>
        );

      }
        
      });

      var MyOption = React.createClass({
        render: function(){
          var selectedStyle = {backgroundColor:'red', color:'#fff', cursor:'pointer'};
          var unSelectedStyle = {cursor:'pointer'};
          if (this.props.value === this.props.state) {
            return <div style={selectedStyle} onClick={this.props.select}>{this.props.value}</div>; //onClick:当点击时执行属性中的select方法。
          } else{
            return <div style={unSelectedStyle} onClick={this.props.select}>{this.props.value}</div>;
          }
        }
      });

      ReactDOM.render(
        <MySelect />, 
        document.getElementById("example07")
        );
      
    </script>
```


#React组件属性
组件属性：props
组件属性的功能和HTML attribute类型。props为组件提供配置值。

#验证组件属性
定义组件的时候，propTypes的配置项可以用来验证传递给props的属性值是否符合要求。
基本类型验证

名称 | 属性
------- | -------
React.PropTypes.string | 验证字符串
React.PropTypes.bool | 布尔值
React.PropTypes.func | 函数
React.PropTypes.number | 数字
React.PropTypes.object | 对象
React.PropTypes.array | 数组
React.PropTypes.any | 验证prop是否是不为空的任意类型

必要验证类型
React.PropTypes.[TYPE].isRequired:确保提供和 .isRequired 所要求的验证类型一致的prop(e.g.,propTypes:{propFunc:React.PropTypes.func.isRequired})

元素验证

React.PropTypes.element  | 验证是否是一个React元素 
------- | -------
React.PropTypes.node | 验证是否是数字，字符串，DOM元素，或者包含以上类型的数组 / 片段都可以被渲染

可枚举性验证

React.PropTypes.oneOf(['Mon', 'Fri']) | 是否是具体数值中的一个
------- | -------
React.PropTypes.oneOfType([React.PropTypes.string, React.PropTypes.number]) | 是否是很多类型中的一种

数组或对象验证

React.PropTypes.arrayOf(React.PropTypes.number) | 是否是只包含一种类型的数组
------- | -------
React.PropTypes.objectOf(React.PropTypes.number) | 是否是只包含一种类型的对象
React.PropTypes.instanceOf(People) | 是否是特定构造函数的实例
React.PropTypes.shape({color:React.PropTypes.string, size:React.PropTypes.number}) | 是否是包含特定类型属性的对象

##创建有状态函数组件
当一个组件只有孤单的props，没有state时，组件可以被写成纯函数，从而避免了创建组件实例的需要。

```
var MyComponent = function(props){
	return <div>Hello {props.name}</div>;
};

MyComponent.defaultProps = {name:"John Doe"};
MyComponent.propTypes = {name: React.PropTypes.string};
ReactDOM.render(<MyComponent name="doug" />, document.getElementById('xxx'));
```

构建一个没有调用 React.createClass()的React组件被称为无状态函数组件。
无状态函数组件不能被传递自己选项（render，componentWillUnmount等等）。然而，.propTypes和.defaultProps在函数中可以被作为属性设置。







