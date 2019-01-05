## **一、React子组件和父组件之间的通信是如何实现的**

```
1、子组件获取父组件属性：props或者state
2、子组件调用父组件的方法
3、父组件获取子组件的属性：props或者state
4、父组件调用子组件的方法
```

```
 1 class Child extends Component{
 2     constructor(props) {
 3       super(props);
 4 
 5       this.state = {
 6         name:"ChildName2222"
 7       };
 8       this.getFatherName = this.getFatherName.bind(this);
 9     }
10 
11     static defaultProps = {
12         ChildName: 'ChildName'
13     }
14 
15     Fuc(){
16         alert("调用子组件方法成功");
17     }
18 
19     getName(){
20         return "Props： " + this.props.ChildName + " , State: " + this.state.name;
21     }
22 
23     getFatherName(){
24         alert(this.props.father);
25     }
26 
27     render(){
28         console.log(this.props);
29         // 调用父组件方法
30         // 获取父组件的属性：props或者state
31         return  <div>
32                     <button onClick={this.getFatherName}>获取父组件的属性</button>
33                     <button onClick={this.props.Fuc}>调用父组件方法</button>
34                 </div>;
35     }
36 };
37 
38 class Father extends Component{
39     constructor(props) {
40       super(props);
41       this.state = {
42         fatherName:"father"
43       };
44       this.runChildFuc = this.runChildFuc.bind(this);
45       this.getChildName = this.getChildName.bind(this);
46     }
47 
48     static defaultProps = {
49         fatherName: 'father2222'
50     }
51 
52     Fuc(){
53         alert("调用父组件方法成功!");
54     }
55 
56     runChildFuc(){
57         this.refs["child"].Fuc();
58     }
59 
60     getChildName(){
61         alert(this.refs["child"].getName());
62     }
63 
64     render(){
65         console.log(this.state.fatherName);
66         return  <div>
67                     <button onClick={this.runChildFuc}>调用子组件方法</button>
68                     <button onClick={this.getChildName}>获取子组件的属性</button>
69                     <Child ref="child" father={"State: " + this.state.fatherName + ", Props: " + this.props.fatherName} Fuc={this.Fuc.bind(this)}></Child>
70                 </div>;
71     }
72 };
73 
74 export default Father;
```

## 二、ES6如何设置组建的默认Props

### **方法一：**

```
class Hello extends React.Component{
  ...
  static defaultProps = {
    name: 'GJKLJ'
  }
  ...
}
```

### 　方法二：

```
class Hello extends React.Component{
  ...
}
 
Hello.defaultProps = {
   name: 'BBB' 
}
```

**\*static 属于新语法，如果用 Babel 编译需要安装 npm install --save-dev babel-preset-stage-0***

## **三、react css行内样式如何渲染？**

```
render() {
   var divStyle = {
     width:'100vw',
     height:'10vw',
      background:'pink',
    };
    return (
      <div style={divStyle}>
        <p className="top">dewfwa</p>
      </div>
    );
}
```

