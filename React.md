**React 不使用 HTML，而使用 JSX 。抛弃 DOM，不使用任何 DOM 方法。**

### Demo

**1、ReactDOM.render 是 React 的最基本方法，用于插入 DOM 节点。**

```
ReactDOM.render(
  <h1>Hello, world!</h1>,
  document.getElementById('example')
);
```

**2、JSX语法**

遇到 HTML 标签（以 `<` 开头），就用 HTML 规则解析；遇到代码块（以 `{` 开头），就用 JavaScript 规则解析。

```
var names = ['Alice', 'Emily', 'Kate'];
ReactDOM.render(
  <div>
  {
    names.map(function (name) {
      return <div>Hello, {name}!</div>
    })
  }
  </div>,
  document.getElementById('example')
);
```

**3、React.createClass（{ }）创建组件类。**

组件类最好用一个变量承接，且变量名首字母必须大写。

组件类只能包含一个顶层标签。

组件类的属性可以接受字符串、对象、函数等等。

`getDefaultProps` 方法用来设置组件属性的默认值。

`PropTypes`是组件类的一个属性，用来验证组件实例的属性是否符合要求。

`render` 方法，用于输出组件，return必须。

```
var MyTitle = React.createClass({
  getDefaultProps : function () {
    return {
      title : 'Hello World'
    };
  },
  
  propTypes: {
    title: React.PropTypes.string.isRequired,
  },
  
  render: function() {
     return <h1> {this.props.title} </h1>;
   }
});

var data = 123;
ReactDOM.render(
  <MyTitle title={data} />,
  document.body
);
```

**4、this.props.children**

`this.props.children` 属性，它表示组件实例的所有子节点。

`React.Children.map` react中的循环方法。

```
var NotesList = React.createClass({
  render: function() {
    return (
      <ol>
      {
        React.Children.map(this.props.children, function (child) {
          return <li>{child}</li>;
        })
      }
      </ol>
    );
  }
});

ReactDOM.render(
  <NotesList>
    <span>hello</span>
    <span>world</span>
  </NotesList>,
  document.body
);
```

### 5、虚拟DOM

组件类的DOM是虚拟DOM，只有插入文档之后才会变成真实的DOM， React 中，所有的 DOM 变动，都先在虚拟 DOM 上发生，然后再将实际发生变动的部分，反映在真实 DOM上，可以极大提高网页的性能表现。

获取真实DOM：给DOM节点添加一个`ref` 属性，然后用this.refs.[refName]获取真实DOM。

```
var MyComponent = React.createClass({
  handleClick: function() {
    this.refs.myTextInput.focus();
  },
  render: function() {
    return (
      <div>
        <input type="text" ref="myTextInput" />
        <input type="button" value="Focus the text input" onClick={this.handleClick} />
      </div>
    );
  }
});

ReactDOM.render(
  <MyComponent />,
  document.getElementById('example')
);
```

### 6、状态 this.state

`this.props` 表示那些一旦定义，就不再改变的特性，而 `this.state` 是会随着用户互动而产生变化的特性，即双向数据绑定（但是需要用setState提交状态）。

`this.setState` 方法修改状态值，每次修改以后，自动调用`this.render` 方法，再次渲染组件。

```
var LikeButton = React.createClass({
  getInitialState: function() {
    return {liked: false};
  },
  handleClick: function(event) {
    this.setState({liked: !this.state.liked});
  },
  render: function() {
    var text = this.state.liked ? 'like' : 'haven\'t liked';
    return (
      <p onClick={this.handleClick}>
        You {text} this. Click to toggle.
      </p>
    );
  }
});

ReactDOM.render(
  <LikeButton />,
  document.getElementById('example')
);
```

### 7、表单 —双向数据绑定

文本输入框的值，不能用 `this.props.value` 读取，而要定义一个 `onChange` 事件的回调函数，通过 `event.target.value` 读取用户输入的值。`textarea` 元素、`select`元素、`radio`元素都属于这种情况。

```
var Input = React.createClass({
  getInitialState: function() {
    return {value: 'Hello!'};
  },
  handleChange: function(event) {
    this.setState({value: event.target.value});
  },
  render: function () {
    var value = this.state.value;
    return (
      <div>
        <input type="text" value={value} onChange={this.handleChange} />
        <p>{value}</p>
      </div>
    );
  }
});
ReactDOM.render(<Input/>, document.body);
```

### 8、组件的生命周期

一共三个状态：

- Mounting：已插入真实 DOM
- Updating：正在被重新渲染
- Unmounting：已移出真实 DOM

每个状态两种函数，`will` 函数在进入状态之前调用，`did` 函数在进入状态之后调用，三种状态共计五种处理函数：

- componentWillMount（）
- componentDidMount（）
- componentWillUpdate（object nextProps, object nextState ）
- componentDidUpdate（object prevProps, object prevState ）
- componentWillUnmount（）

两种特殊状态处理函数

- componentWillReceiveProps（object nextProps ）已加载组件收到新的参数时调用
- shouldComponentUpdate（object nextProps, object nextState）组件判断是否重新渲染时调用

**组件的style属性的设置方式值得注意，这是因为React 组件样式是一个对象，所以第一重大括号表示这是 JavaScript 语法，第二重大括号表示样式对象。**

```
var Hello = React.createClass({
  // 设置初始值
  getInitialState: function () {
    return {opacity: 1.0};
  },
  // 状态改变
  componentDidMount: function () {
    this.timer = setInterval(function () {
      var opacity = this.state.opacity;
      opacity -= .05;
      if (opacity < 0.1) { opacity = 1.0;}
      this.setState({opacity: opacity});
    }.bind(this), 100);
  },
  // 组件渲染
  render: function () {
    return (
      <div style={{opacity: this.state.opacity}}>
        Hello {this.props.name}
      </div>
    );
  }
});
 // 组件插入DOM
ReactDOM.render(
  <Hello name="world"/>, document.body
);
```

## react-native脚手架

```
npm install -g create-react-native-app 
create-react-native-app my-app
cd my-app/
npm start
```

### react脚手架

```
npm install -g create-react-app
create-react-app my-app
cd my-app/
npm start
```

## React的设计理念为：万物皆组件

### VueJs、ReactJs 皆为声明式渲染

```
react-router React Router 核心

react-router-dom 用于 DOM 绑定的 React Router

react-router-native 用于 React Native 的 React Router

react-router-redux React Router 和 Redux 的集成

react-router-config 静态路由配置的小助手
```

### 一、引用

在 React 的使用中，我们一般要引入两个包，`react` 和 `react-dom` 。

`react-router` 和 `react-router-dom` 只要引用一个就行了。

后者比前者多出了 `<Link>` `<BrowserRouter>` 这样的 DOM 类组件。 **因此我们只需引用 react-router-dom 这个包就行了。**

如果搭配 redux ，你还需要使用 `react-router-redux`。

### 二、组件

**<BrowerRouter>** 使用了 Html5 History Api