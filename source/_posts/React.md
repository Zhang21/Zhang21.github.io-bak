---
title: React
date: 2021-08-06 09:02:17
mathjax: true
categories:
  - Frontend
tags:
  - React
---

参考:

- React docs: <https://zh-hans.reactjs.org/docs/getting-started.html>
- React github: <https://github.com/facebook/react/>

<br>

版本:

- React: v17.0


<br>
<!--more-->
<br>



# 预备知识

React是一个JavaScript库。所以你应该了解:

- JavaScript
- Node
- npm
- yarn




<br>

---

<br>



# 教程

links: <https://zh-hans.reactjs.org/tutorial/tutorial.html>

教程适用于边做边学的开发者。


<br>
<br>


## 环境准备

- 浏览器
- 本地开发环境: 安装Node.js


<br>
<br>


## 概览

React是一个声明式，高效且灵活的用于构建用户界面的JavaScript库。使用React可以将一些简短、独立的代码片段组合成复杂的UI界面，这些代码片段称为**组件**。

大多数的React开发者使用一种名为**JSX**的特殊语法，JSX可以让你更轻松地书写这些结构。
在JSX中你可以任意使用JavaScript表达式，只需要用一个大括号把表达式括起来。每一个React元素事实上都是一个JavaScript对象，你可以在你的程序中把它保存在变量中或者作为参数传递。

**箭头函数表达式**的语法比函数表达式更简洁。函数箭头表达式更适合用于那些本来需要匿名函数的地方，并且它不能作为构造函数。

```javascript
<button className="square" onClick={function() { alert('click'); }}>
</button>

<button className="square" onClick={() => alert('click)}>
</button>
```



<br>

---

<br>



# 核心概念


<br>


## HelloWorld

```react
ReactDOM.render(
  <h1>HelloWorld!</h1>
  document.getElementById('root')
);
```


<br>
<br>


## JSX简介

https://zh-hans.reactjs.org/docs/introducing-jsx.html

建议在 React 中配合使用 JSX，JSX 可以很好地描述 UI 应该呈现出它应有交互的本质形式。JSX 可能会使人联想到模板语言，但它具有 JavaScript 的全部功能。

React 不强制要求使用 JSX，但是大多数人发现，在 JavaScript 代码中将 JSX 和 UI 放在一起时，会在视觉上有辅助作用。它还可以使 React 显示更多有用的错误和警告消息。


<br>
<br>


### 在JSX中嵌入表达式

声明一个name变量，然后在JSX中使用它，并将它包裹在大括号中：

```javascript
const name = 'Josh Perez';
const element = <h1>Hello, {name}</h1>;

ReactDOM.render(
  element,
  document.getElementById('root)
);
```

在JSX语法中，你可以在大括号中防止任何有效的JavaScript表达式。

```javascript
function formatName(user) {
  return user.firstName + ' ' + user.lastName;
}

const user = {
  <h1>
    Hello, {formantName(user)}!
  </h1>
};

ReactDOM.renger(
  element,
  document.getElementById('root')
);
```

为了便于阅读，将JSX拆分为多行。同时，建议将内容包裹在括号中，虽然这样做不是强制要求，但这可以避免遇到自动插入分号陷阱。

<br>
<br>


### JSX也是一个表达式

在编译之后，JSX 表达式会被转为普通 JavaScript 函数调用，并且对其取值后得到 JavaScript 对象。

```javascript
function getGreeting(user) {
  if (user) {
    return <h1>Hello, {formantName(user)}!</h1>;
  }
  return <h1>Hello, Stranger.</h1>;
}
```


<br>
<br>


### JSX特定属性

通过使用引号，来将属性值指定为字符串字面量。也可以使用大括号，在属性值中插入一个JavaScript表达式：

```javascript
const element = <div tabIndex="0"></div>;
const element = <img src={user.avatarUrl}></img>;
```

因为JSX语法上更接近JavaScript而不是HTML，所以React DOM使用驼峰命名来定义属性的名称。


<br>
<br>


### 使用JSX指定子元素

假如一个标签里没有内容，你可以使用`/>`来闭合标签，就像XML语法一样。JSX标签里能够包含很多子元素。

```javascript
const element = <img src={user.avatarUrl} />;

const element = (
  <div>
    <h1>Hello!</h1>
    <h2>Good to see you here.</h2>
  </div>
)
```


<br>
<br>


### JSX防注入攻击

你可以安全地在JSX当中插入用户输入内容。

```javascript
const title = response.potentiallyMaliciousInput;
// 直接使用是安全的
const element = <h1>{title}</h1>;
```

React DOM在渲染所有输入内容之前，默认会进行转义。它可以确保在你的应用中，永远不会注入那些并非自己明确编写的内容。所有的内容在渲染之前都被转换成了字符串。这样可以有效防止XSS攻击。


<br>
<br>


### JSX表示对象

Babel 会把 JSX 转译成一个名为 React.createElement() 函数调用。



<br>

---

<br>



## 元素渲染

https://zh-hans.reactjs.org/docs/rendering-elements.html

元素是构成React应用的最小砖块。元素描述了你在屏幕上想看到的内容。

与浏览器的DOM元素不通，React元素是创建开销极小的普通对象。React DOM会负责更新DOM来与React元素保持一致。


<br>


### 将一个元素渲染为DOM


<br>
<br>


### 更新已渲染的元素

React元素是不可变对象。一旦被创建，你就无法更改它的子元素或者属性。

根据我们已有的知识，更新 UI 唯一的方式是创建一个全新的元素，并将其传入 `ReactDOM.render()`。

在实践中，大多数React应用只会调用一次`ReactDOM.render()`。


<br>
<br>


### 只更新它需要更新的部分

React DOM 会将元素和它的子元素与它们之前的状态进行比较，并只会进行必要的更新来使 DOM 达到预期的状态。



<br>

---

<br>



## 组件和props

https://zh-hans.reactjs.org/docs/components-and-props.html

组件允许你将UI拆分为独立可复用的代码片段，并对每个片段进行独立构思。

组件，从概念上类似于JavaScript函数。它接受任意的入参(props)，并返回用于描述页面展示内容的React元素。

<br>

### 函数组件和class组件

定义组件最简单的方式就是编写 JavaScript 函数。

```javascript
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

// 或ES6 class
class Welcom extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```

这两个组件在React里是等效的。


<br>
<br>


### 渲染组件

React也可以是用户自定义的组件。

```javascript
const element = <Welcome name="Sara" />;
```

当React元素为用户自定义组件时，它会将JSX所接收的属性以及子组件转换为单个对象传递给组件，这个对象称位**props**。

```javascript
// Hello, Sara

function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

const element = <Welcome name="Sara" />;
ReactDOM.render(
  element,
  document.getElementById('root')
);
```

组件名称必须以大写字母开头。React会将以小写字母开头的组件视为原生DOM标签。


<br>
<br>


### 组合组件

组件可以在其输出中引用其它组件。

例如，我们可以创建一个多次渲染Welcome组件的App组件。

```javascript
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

function App() {
  return (
    <div>
      <Welcome name="A" />
      <Welcome name="B" />
      <Welcome name="C" />
    </div>
  );
}

ReactDOM.render(
  <App />,
  document.getElementById('root)
);
```


<br>
<br>


### 提取组件

将组件拆分为更小的组件。

最初看上去，提取组件可能是一件繁重的工作，但是，在大型应用中，构建可复用组件库是完全值得的。根据经验来看，如果UI中有一部分被多次使用(Button, Panel, Avatar)，或者组件本身就足够复杂，那么它就是一个可提取出独立组件的候选项。


<br>
<br>

### Props只读性

组件无论是使用函数声明还是class声明，都绝不能修改自身的props。

React非常灵活，但它也有一个严格的规则。**所有React组件都必须像纯函数一样保护它们的props不被更改**。

<br>

> 备注:<br>
> 纯函数: 不会尝试修改入参


<br>

---

<br>


## State和生命周期

文档: <https://zh-hans.reactjs.org/docs/state-and-lifecycle.html>

前面，我们只了解了通过`ReactDOM.render()`来修改我们想要渲染的元素。在本章节中，我们将学习如何封装真正可复用的Clock组件。它将设置自己的计时器并每秒更新一次。

我们需要使用**state**来实现这个功能。state与props类似，但state是私有的，并且完全受控于当前组件。


<br>
<br>


### 将函数组件转换为class组件

通过以下五步将Clock的函数组件转换成class组件：

1. 创建一个同名的ES6 class，并且继承于`React.Component`
2. 添加一个空的`render()`方法
3. 将函数体移动到`render()`方法之中
4. 在`render()`方法中使用`this.props`替换`props`
5. 删除剩余的空函数声明

```javascript
class Clock extends React.Component {
  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.props.date.toLocalTimeString()}</h2>
      </div>
    );
  }
}
```


<br>
<br>


### 向class组件中添加局部的state

把`render()`方法中的`this.props.date`替换成`this.state.date`:

```javascript
class Clock extends React.Component {
  render() {
    return (
      <div>
        <h1>Hello, world</h1>
        <h2>It is {this.state.date.toLocalTimeString()}</h2>
      </div>
    );
  }
}
```

<br>

添加一个class构造函数（将props传递到父类的构造函数中），然后在该函数中为`this.state`赋初值:

```javascript
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }

  render () {
    ...
  }
}
```

Class组件应该始终使用props参数来调用父类的构造函数。

<br>

移除`<Clock />`元素中的date属性:

```javascript
ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);
```


<br>
<br>


### 将生命周期方法添加到Class中

在具有许多组件的应用程序中，当组件被销毁时释放所占用的资源是非常重要的。

当Clock组件第一次被渲染到DOM中的时候，就为其设置一个计时器。这在React中被称为**挂载(mount)**。
同时，当DOM中Clock组件被删除的时候，应该清除计时器。这在React中被称为**卸载(unmount)**。

我们可以为class组件声明一些特殊的方法，当组件挂载或卸载时就会去执行这些方法，这些方法叫做**生命周期方法**。

尽管`this.props`和`this.state`是React本身设置的，且都拥有特殊的含义，但其实你可以向class中随意添加不参与数据流的额外字段（如计时器ID）。

```javascript
class Clock extends React.Component {
  constructor(props) {
    ...
  }

  componentDidMount() {
    this.timerID = setInterval(
      () => this.tick(),
      1000
    );
  }

  componentWillUnmount() {
    clearInterval(this.timerID);
  }

  tick() {
    this.setState({
      date: new Date()
    });
  }

  render() {
    ...
  }
}

ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);
```


<br>
<br>


### 正确地使用State

关于`setState()`你应该了解三件事：

- 不要直接修改State
- State的更新可能是异步的
- State的更新会被合并

```javascript
// Wrong
this.state.comment = "Hello";
// Correct
this.setState({comment: "Hello"});


// Wrong
this.setState({
  counter: this.state.counter + this.props.increment,
});
// Correct
this.setState((state, props) =>({
  counter: state.counter + props.increment
}));
```


<br>
<br>


### 数据是向下流动的

不管是父组件还是子组件都无法知道某个组件是有状态的还是无状态的，并且它们也并不关心它是函数组件还是class组件。

这就是为什么称state为局部的或是封装的原因。除了拥有设置了它的组件，其它组件都无法访问。

组件可以选择把它的state作为props向下传递到它的子组件中。

这通常被叫做自上而下或是单向的数据流。任何的state总是所属于特定的组件，而且从改state派生的任何数据或UI只能影响树中低于它们的组件。


<br>

---

<br>


## 事件处理

React元素的事件处理和DOM元素的很相似，但有一点语法上的不同：

- React事件的命名使用驼峰法，而不是纯小写
- 使用JSX语法时你需要传入一个函数作为事件处理函数，而不是一个字符串

```javascript
// 传统的HTML
<button onclick="activateLasers()">
  Activate Lasers
</button>


// React中
<button onClick={activateLasers}>
  Activate Lasers
</button>
```


<br>
<br>


### 向事件处理程序传递参数

在循环中，通常我们会为事件处理函数传递额外的参数。


<br>

---

<br>



## 条件渲染

文档: <https://zh-hans.reactjs.org/docs/conditional-rendering.html>

在React中，你可以创建不同的组件来封装各种你需要的行为。然后，依据应用的不同状态，你可以只渲染对应状态下的部分内容。


<br>
<br>


### 元素变量

使用变量来存储元素。它可以帮助你有条件地渲染组件的一部分，而其它的渲染部分并不会因此而改变。


<br>
<br>


### 运算符

```
与运算符: &&
三目运算符: condition ? true : false
```

因为在JavaScript中，`true && expression`总是会返回`expression`, 而`false && expression`总是会返回`false`。因此，如果条件是`true`，`&&`右侧的元素就会被渲染，如果是`false`，React会忽略并跳过它。


<br>
<br>


### 阻止组件渲染

在某些情况下，你可能希望隐藏组件，即使它已经被其它组件渲染。若要完成此操作，你可以让`render`方法直接放回`null`，而不进行任何渲染。


<br>

---

<br>


## 列表和Key

文档: <https://zh-hans.reactjs.org/docs/lists-and-keys.html>

使用`map()`函数让数组中的每一项变双倍，然后我们得到一个新的列表。

```javascript
const numbers = [1, 2, 3, 4, 5];
const doubled = numbers.map((number) => number * 2);
console.log(doubled);
```

在React中，把数组转化为元素列表的过程是相似的。


<br>
<br>


### 渲染多个组件

可以通过使用花括号`{}`在JSX内构建一个元素集合。

```javascript
const numbers = [1, 2, 3, 4, 5];
const listItems = numbers.map((number) =>
  <li>{number}</li>
);

ReactDOM.renger(
  <ul>{listItems}</ul>,
  document.getElementById('root')
);
```


<br>
<br>


### 基础组件列表

通常你需要在一个组件中渲染列表。

我们可以把前面的例子重构成一个组件，这个组件接收numbers数组作为参数并输出一个元素列表。

```javascript
function NumberList(props) {
  const numbers = props.numbers;
  const listIterms = numbers.map((number) =>
    <li key={number.toString()}>
      {number}
    </li>
  );
  return (
    <ul>{listItems}</ul>
  );
}

const numbers = [1, 2, 3, 4, 5]
ReactDOM.render(
  <NumberList numbers={numbers} />,
  document.getElementById('root')
);
```


<br>
<br>


### key

key帮助React识别那些元素改变了，比如被添加或删除。因此你应当给数组中的每一个元素赋予一个确定的标识。

一个元素的key最好是这个元素在列表中拥有的一个独一无二的字符串。通常，我们使用数据中的id来作为元素的key。


<br>
<br>


### 用key提取组件

元素的key只有放在就近的数组上下文中才有意义。

比方说，如果你提取出一个ListItem组件，你应该把key保留在数组中的这个`<ListItem />`元素上，而不是放在ListItem组件的`<li>`元素上。

```javascript
// 正确！key 应该在数组的上下文中被指定
<ListItem key={number.toString()} value={number} />
```

一个好的经验法则，在`map()`方法中的元素需要设置key属性。


<br>
<br>


### key只是在兄弟节点之间必须唯一

数组元素中使用的key在其兄弟节点之间应该是独一无二的。然而，它们不需要是全局唯一的。


<br>
<br>


### 在JSX中嵌入map()

JSX允许在大括号中嵌入任何表达式，所以我们可以内联`map()`返回的结果。

```javascript
function NumberList(props) {
  const numbers = props.numbers;
  return (
    <ul>
      {numbers.map((number) =>
        <ListItem key={number.toString()}
                  value={number} />
      )}
    </ul>
  );
}
```


<br>

---

<br>


## 表单

文档: <https://zh-hans.reactjs.org/docs/forms.html>

在React里，HTML表单元素的工作方式和其它的DOM元素有些不同，这是因为表单元素通常会保持一些内部的state。

在大多数情况下，使用JavaScript函数可以很方便的处理表单的提交，同时还可以访问用户填写的表单数据。实现这种效果的标准方式是使用**受控组件**。


<br>
<br>


### 受控组件

在HTML中，表单元素通常自己维护state，并根据用户输入进行更新。而在React中，可变状态通常保存在组件的state属性中，并且只能通过使用`setState()`来更新。

我们可以把两者结合起来，是React的state成为唯一数据源。渲染表单的React组件还控制着用户输入过程中表单发生的操作。被React以这种方式控制取值的表单输入元素就叫做受控组件。


<br>
<br>


### textarea标签

在HTML中，`<textarea>`元素通过其子元素定义其文本。

而在React中，`<textarea>`使用`value`属性代替。这样可以使得使用`<textarea>`的表单和使用单行input的表单非常类似。


<br>
<br>


### select标签

在HTML中，`<select>`创建下拉列表标签。

<br>

> **注意:**<br>
> 你可以将数组传递到value属性中，以支持在select标签中选择多个选项<br>
> `<select multiple={true} value={['B', 'C']}>`


<br>
<br>


### 文件input标签

在HTML中，`<input type="file">`允许用户从存储设备中选择一个或多个文件，将其上次到服务器，或通过JavaScript的File API进行控制。

因为它的value只读，所以它是React中的一个非受控组件。


<br>
<br>


### 处理多个输入

当需要处理多个input元素时，我们可以给每个元素添加name属性，并让处理函数根据`event.target.name`的值选择要执行的操作。


<br>
<br>


### 受控输入空值

在受控组件上指定value的prop会阻止用户更改输入。如果你指定了value，但输入仍可编辑，则可能是你意外地将value设置为undefined或null。


<br>

---

<br>


## 状态提升

文档: <https://zh-hans.reactjs.org/docs/lifting-state-up.html>

通常，多个组件需要反映相同的变化数据，这时我们建议将共享状态提升到嘴贱的共同父组件中去。


<br>
<br>


### 学习小结

在React应用中，任何可变数据应当只有一个相对应的唯一数据源。通常，state都是首先添加到需要渲染数据的组件中去。然后，如果其它组件也需要这个state，那么你可以将它提升至这些组件的最近共同父组件中。你应当依靠自上而下的数据流，而不是尝试在不同组件间同步state。


<br>

---

<br>


## 组合和继承

文档: <https://zh-hans.reactjs.org/docs/composition-vs-inheritance.html>

React有十分强大的组合模式。我们推荐使用组合而非继承来实现组件间的代码重用。


<br>
<br>


### 包含关系

有些组件无法提前知晓它们子组件的具体内容。在侧边栏(sidebar)和对话框(dialog)等展现通用容器(box)的组件中特别容易遇到这种情况。

我们建议这些组件使用一个特殊的children prop来将它们的子组传递到渲染结果中。

```javascript
function FancyBorder(props) {
  return (
    <div className={'FancyBorder FancyBorder-' + props.color}>
      {props.children}
    </div>
  );
}
```

这使得别的组件可以通过JSX嵌套，将任意组件作为子组件传递给它们。

```javascript
function WelcomDialog() {
  return (
    <FancyBorder color="blue">
      <h1 className="Dialog-title">
        Welcome
      </h1>
      <p className="Dialog-message>
        Thank you for visiting our spacecraft!
      </p>
    </FancyBorder>
  );
}
```


<br>
<br>


### 特例关系

有些时候，我们会把一些组件看作是其它组件的特殊实例。比如WelcomeDialog可以说是Dialog的特殊实例。

在React中，我们可以通过组合来实现这一点。特殊组件可以通过props定制并渲染一般组件。

```javascript
function Dialog(props) {
  return (
    <FancyBorder color="blue">
      <h1 className="Dialog-title">
        {props.title}
      </h1>
      <p className="Dialog-message">
        {props.message}
      </p>
      {props.children}
    </FancyBorder>
  );
}

class SignUpDialog extends React.Component {
  constructor(props) {
    super(props);
    this.handleChange = this.handleChange.bind(this);
    this.handleSignUp = this.handleSignUp.bind(this);
    this.state = {login: ''};
  }

  render() {
    return (
      <Dialog title="Mars Exploration Program"
              message="How should we refer to you?">
        <input value={this.state.login}
               onChange={this.handleChange} />
        <button onClick={this.handleSignUp}>
          Sign Me Up!
        </button>
      </Dialog>
    );
  }

  handleChange(e) {
    this.setState({login: e.target.value});
  }

  handleSignUp() {
    alert(`Welcome aboard, ${this.state.login}!`);
  }
}
```


<br>
<br>


### 继承

在Facebook，我们在成百上千个组件中使用React。我们并没有发现需要使用继承来构建组件层次的情况。

props和组合为你提供了清晰而安全地定制组件外观和行为的灵活方式。组件可以接受任意props，包括基本数据类型，React元素以及函数。


<br>
<br>


## React哲学

文档: <https://zh-hans.reactjs.org/docs/thinking-in-react.html>

我们认为，React是用JavaScript构建快速响应的大型web应用程序的首选方式。React最棒的部分之一是引导我们思考如何构建一个应用。

<br>

从设计稿开始：

- 将设计好的UI划分为组件层级
- 用React创建一个静态版本
- 确定UI state的最小且完整表示
- 确定state放置的位置
- 添加反向数据流










<br>

---

<br>


























# 高级指引




























































<br>

---

<br>












# FAQ

文档: <https://zh-hans.reactjs.org/docs/faq-state.html>


















