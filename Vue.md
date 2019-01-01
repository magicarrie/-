## 一、基础阶段

- Vue的初始化：new Vue({});一个new方法，里面的参数是一个对象，对象里面的'el'属性指向作用元素，'data'属性用于设置所绑定的数据，'methods'用于设置元素的绑定的事件与方法。

- 注：el使用‘body’无效

- v-bind:’属性‘=“属性值 ”；给元素设置**属性** 。

- ​

- v-if='布尔值'；控制一个元素的显示与隐藏。

- v-for='li in list':处理循环

- v-on:click='function':绑定事件

- 数组转换成字符串：join('分隔符');字符串转换成数组：split('分隔符')。

- v-model:双向数据绑定 v-model="message"

- 组件：自定义元素并引用

- v-html:插入html

- html中可以写JavaScript表达式{{}}

- v-if中添加js语句

  ```
  <div v-if="Math.random() > 0.5">
    Sorry
  </div>
  <div v-else>
    Not sorry
  </div>
  ```

## 二、进阶阶段

#### 1.Vue的响应式系统

（1）vue会在初始化实例时，对属性执行getter/setter转化过程，所以属性必须在data对象上存在，vue才能转化他，这样这个属性才能使响应式的。vue不允许在已经创建的实例上动态添加新的根级响应式属性。

修改属性：vm.a=2;

获取属性：vm.a;

（2）可以使用Vue.set(vm.someObject,key,value)将响应属性添加到vm中嵌套的对象上。

```
Vue.set(vm.num, 'b', 2)
console.log(vm.$data.num.b);
```

（3）向已有对象上添加一些属性，例如使用 Object.assign()或 _.extend() 方法来添加属性。但是，添加到对象上的新属性不会触发更新。在这种情况下可以创建一个新的对象，让它包含原对象的属性和新的属性：

```
this.someObject = Object.assign({}, this.someObject, { a: 1, b: 2 })
```

## 三、测试阶段

1.mint-ui 插件ios系统上，弹出异常，没有完全弹出

```
解决办法：跟插件有关的input标签换成span标签；
```

2.ios系统mint-ui插件，选择日期时，点击确定会出现NAN

```
解决办法：插件所绑定的日期必须为对象格式，即 new Date("年-月-日");且月份和日期<10时，前面必须加0；如：4月份，应该为04
```

3.iphone7,10.3.1版本mint插件选择确定日期时日期显示有误，会显示两个日期，原来的日期也会显示出来

```
span标签换成P标签，并且display为inlineBlock时有误，block时正常
```

4.ios部分机型低版本不支持Vue，主要是8.1及9.3

```
vue全部乱套，methodes里面的函数没有解析，把vue式的函数声明改为常规的函数声明 即括号前面加一个function的声明
例如 ： CheckForm (){}改为 CheckForm ：function ()
```

5.数据没有渲染进来时，出现短暂的双大括号{{}}

```
数据的绑定形式{{}}改为v-text
```

6.vue的ajax不支持同步，只能异步;一般的ajax都是异步的，但是短信校验需要同步

7.ios系统mint-ui,date插件，选择日期时会有冒泡事件，插件滑动时，下面的页面也会跟着滑动，严重时，不能工作

```
插件用div包起来，div绑定touchmove事件；事件发生函数为
function(e){
  var event = window.event  || e;
  event.preventDefault();
}
```
如何设置组件的默认props

**ES6的写法：**

static 属于新语法，用 Babel 编译需要安装 npm install --save-dev babel-preset-stage-0

**方法一：**

```
class Hello extends React.Component{
  ...
  static defaultProps = {
    name: 'GJKLJ'
  }
  ...
}
```

**方法二：**

```
class Hello extends React.Component{
  ...
}

Hello.defaultProps = {
   name: 'BBB'  
}
```