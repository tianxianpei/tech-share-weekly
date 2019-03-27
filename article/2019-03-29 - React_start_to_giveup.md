# React 从入门到放弃

## 写 UI 的 JavaScript 库

- 声明式设计 −React 采用声明范式，可以轻松描述应用。

- 高效 −React 通过对 DOM 的模拟，最大限度地减少与 DOM 的交互。

- 灵活 −React 可以与已知的库或框架很好地配合。

- JSX − JSX 是 JavaScript 语法的扩展。React 开发不一定使用 JSX，但建议使用它。

- 组件 − 通过 React 构建组件，使得代码更加容易得到复用，能够很好的应用在大项目的开发中。

- 单向响应的数据流 − React 实现了单向响应的数据流，从而减少了重复代码，这也是它为什么比传统数据绑定更简单。

#### HTML ( HyperText Markup Language ) ???

#### CSS ( Cascading Style Sheets ) ???

#### JavaScript ???

## 安装&启动

- 1.  [Node.js](https://nodejs.org/dist/v10.15.3/node-v10.15.3.pkg)
- 2. `npm i -g create-react-app`
     `cnpm i -g create-react-app`
     `yarn global add create-react-app`
- 3. `create-react-app xxxx`
- 4. `cd xxxx`
- 5. `npm run start`

## JSX

_A syntax extension to JavaScript._

```jsx
const element = <h1>Hello, world!</h1>;`
```

这种看起来可能有些奇怪的标签语法既不是字符串也不是 HTML。它被称为 JSX， 一种 JavaScript 的语法扩展。 推荐在 React 中使用 JSX 来描述用户界面。
JSX 是在 JavaScript 内部实现的。元素是构成 React 应用的最小单位，JSX 就是用来声明 React 当中的元素。
与浏览器的 DOM 元素不同，React 当中的元素事实上是普通的对象，React DOM 可以确保 浏览器 DOM 的数据内容与 React 元素保持一致。
要将 React 元素渲染到根 DOM 节点中，通过把它们都传递给 ReactDOM.render() 的方法来将其渲染到页面上：

```jsx
const name = 'Neo'
const element = <h1 className="heading">Hello, {name}</h1>

ReactDOM.render(element, document.getElementById('root'))
```

_由于 JSX 基本可以认为就是 JavaScript，一些标识符像 class 和 for 不建议作为 XML 属性名。作为替代，React DOM 使用 className 和 htmlFor 来做对应的属性。_

#### 条件渲染

`if/else`
`a ? b : c`
`true && xxxxx`

#### 列表渲染

```jsx
Array.map((content, index) => <div key={content.id}>{{ content }}</div>)
```

每个列表元素必须分配一个 key。key 可以在 DOM 中的某些元素被增加或删除的时候帮助 React 识别哪些元素发生了变化。因此应当给数组中的每一个元素赋予一个确定的标识。

一个元素的 key 最好是这个元素在列表中拥有的一个独一无二的字符串。通常使用来自数据的 id 作为元素的 key。

当元素没有确定的 id 时，可以使用它的序列号索引 index 作为 key。

如果列表可以重新排序，不建议使用索引来进行排序，因为这会导致渲染变得很慢。

## Component

_组件使得应用更容易管理_

- 使用函数定义了一个组件

```jsx
function HelloMessage() {
  return <h1>Hello World !</h1>
}

ReactDOM.render(<HelloMessage />, document.getElementById('example'))
```

- 使用 ES6 class 来定义一个组件

```jsx
class HelloMessage extends React.Component {
  render() {
    return <h1>Hello World!</h1>
  }
}
```

- 复合组件

```jsx
function Name() {
  return <h1>Neo</h1>
}

function HelloMessage() {
  return (
    <div>
      Hello World, <Name />!
    </div>
  )
}
```

## State

React 把组件看成是一个状态机（State Machines）。通过与用户的交互，实现不同状态，然后渲染 UI，让用户界面和数据保持一致。

React 里，只需更新组件的 state，React 会重新渲染用户界面（一般不操作 DOM）。

```jsx
class Clock extends React.Component {
  constructor(props) {
    super(props)

    this.state = {
      name: 'Neo',
      time: new Date()
    }
  }

  componentDidMount() {
    this.timerID = setInterval(() => this.updateTime(), 1000)
  }

  componentWillUnmount() {
    clearInterval(this.timerID)
  }

  updateTime() {
    this.setState({
      time: new Date()
    })
  }

  render() {
    return (
      <div>
        <h1>Hello, world, {this.state.name}!</h1>
        <div>{this.state.time.toLocaleTimeString()}</div>
      </div>
    )
  }
}
```

#### 单向数据流

- 任何状态始终由某些特定组件所有，并且从该状态导出的任何数据或 UI 只能影响树中下方的组件。

```jsx
class Clock extends React.Component {
  constructor(props) {
    super(props)

    this.state = {
      name: 'Neo',
      time: new Date()
    }
  }

  componentDidMount() {
    this.timerID = setInterval(() => this.updateTime(), 1000)
  }

  componentWillUnmount() {
    clearInterval(this.timerID)
  }

  updateTime() {
    this.setState({
      time: new Date()
    })
  }

  render() {
    return (
      <div>
        <h1>Hello, world, {this.state.name}!</h1>
        <TimeShow time={this.state.time} />
      </div>
    )
  }
}

function TimeShow(props) {
  return props.time && props.time.toLocaleTimeString ? (
    <div>{props.time.toLocaleTimeString()}</div>
  ) : null
}
```

想象一个组件树作为属性的瀑布，每个组件的状态就像一个额外的水源，它连接在一个任意点，但也会向下流动。

## Props

用来传递数据给子组件

```jsx
function HelloMessage(props) {
  return <h1>Hello {props.name}!</h1>
}

ReactDOM.render(<HelloMessage name="Neo" />, document.getElementById('example'))
```

## 事件处理

- 事件绑定属性的命名采用驼峰式写法

- 在 JSX 中，需要传入一个函数作为事件处理函数

```html
<!-- html event -->
<button onclick="handleClick()">
  激活按钮
</button>
```

```jsx
// jsx event
<button onClick={handleClick}>激活按钮</button>
```

#### 谨慎对待类的方法里 JSX 回调函数中的 this

类的方法默认是不会绑定 this 的。如果忘记绑定 this.handleClick 并把它传入 onClick, 当调用这个函数的时候 this 的值会是 undefined。

```jsx
class Toggle extends React.Component {
  constructor() {
    super()
    this.state = {
      isOn: false
    }
  }

  handleClickToggle() {
    // this is undefined
    // Uncaught TypeError: Cannot read property 'setState' of undefined
    this.setState({
      isOn: !this.state.isOn
    })
  }

  render() {
    return (
      <button onClick={this.handleClickToggle}>
        {this.state.isOn ? 'on' : 'off'}
      </button>
    )
  }
}
```

- 解法一：在类初始化 ( constructor ) 的时候 bind this

```jsx
class Toggle extends React.Component {
  constructor() {
    super()
    this.state = {
      isOn: false
    }

    this.handleClickToggle = this.handleClickToggle.bind(this)
  }

  handleClickToggle() {
    this.setState({
      isOn: !this.state.isOn
    })
  }

  render() {
    return (
      <button onClick={this.handleClickToggle}>
        {this.state.isOn ? 'on' : 'off'}
      </button>
    )
  }
}
```

- 解法二：使用箭头函数定义回调函数

```jsx
class Toggle extends React.Component {
  constructor() {
    super()
    this.state = {
      isOn: false
    }

    // this.handleClickToggle = this.handleClickToggle.bind(this)
  }

  // handleClickToggle() {
  //   this.setState({
  //     isOn: !this.state.isOn
  //   })
  // }

  handleClickToggle = () => {
    this.setState({
      isOn: !this.state.isOn
    })
  }

  render() {
    return (
      <button onClick={this.handleClickToggle}>
        {this.state.isOn ? 'on' : 'off'}
      </button>
    )
  }
}
```

- 解法三：在匿名箭头函数中 call 回调函数

```jsx
class Toggle extends React.Component {
  constructor() {
    super()
    this.state = {
      isOn: false
    }
  }

  handleClickToggle() {
    this.setState({
      isOn: !this.state.isOn
    })
  }

  render() {
    return (
      <button onClick={() => this.handleClickToggle()}>
        {this.state.isOn ? 'on' : 'off'}
      </button>
    )
  }
}
```

## 组件生命周期

- `componentWillMount`
  在渲染前调用,在客户端也在服务端。

* `componentDidMount`
  在第一次渲染后调用，只在客户端。之后组件已经生成了对应的 DOM 结构。 可以在这个方法中调用 setTimeout, setInterval 或者发送 API 请求等操作(防止异步操作阻塞 UI)。

- `componentWillReceiveProps`
  在组件接收到一个新的 prop (更新后)时被调用。这个方法在初始化 render 时不会被调用。

* `shouldComponentUpdate`
  返回一个布尔值。在组件接收到新的 props 或者 state 时被调用。在初始化时或者使用 forceUpdate 时不被调用。 可以在确认不需要更新组件时使用。

- `componentWillUpdate`
  在组件接收到新的 props 或者 state 但还没有 render 时被调用。在初始化时不会被调用。

* `componentDidUpdate`
  在组件完成更新后立即调用。在初始化时不会被调用。

- `componentWillUnmount`
  在组件从 DOM 中移除之前立刻被调用。

## Refs

一般用来取 DOM

```jsx
<input type="text" ref="myInput" />
// this.refs.myInput
```

## React Router ???

## Ant Design

企业级的 UI 框架

[Document](https://ant.design/index-cn)

### 安装

`npm i --save antd`
`cnpm i --save antd`
`yarn add antd`

修改`src/App.jsx`：

```jsx
import React, { Component } from 'react'
import Button from 'antd/lib/button'
import './App.css'

class App extends Component {
  
  ...

  render() {
    return (
      <div className="App">
        ...
        <Button type="primary">Button</Button>
        ...
      </div>
    )
  }
}

...
```

修改`src/App.css`：

```css
@import '~antd/dist/antd.css';
...
```

好了，Ant Design中的所有组件已经可以随意使用了。可以入天鲜配后台前端的坑了。

- 想要写个输入框 ? =====> [Input](https://ant.design/components/input-cn)
* 想要写个表格 ? =======> [Table](https://ant.design/components/table-cn)
- 想要写个提示 ? =====> [Alert](https://ant.design/components/alert-cn)
* 想要写个对话框 ? =====> [Modal](https://ant.design/components/modal-cn)
- .......这里有所有你可以使用的[组件](https://ant.design/docs/react/introduce-cn)

## React Router

完整的 React 路由解决方案 ~~ 官宣的！它能让保持 UI 与 URL 同步 ！ 

### Guide
推荐 React Router 官方入门教程 [React Training -- react-router](https://reacttraining.com/react-router/web/guides/quick-start)。

#### 安装
- `npm i --save react-router-dom`
- `cnpm i --save react-router-dom`
- `yarn add react-router-dom`

#### 最最最最最基础的用法

```jsx
import React from 'react';
import { BrowserRouter, Route, Link } from 'react-router-dom';

function Home() {
  return <div>Home</div>
}

function User() {
  return <div>User</div>
}

export default function App() {
  return (
    <BrowserRouter>
      <nav>
        <ul>
          <li>
            <Link to="/">Home</Link>
          </li>
          <li>
            <Link to="/user">User</Link>
          </li>
        </ul>
      </nav>

      <hr />

      <Route exact path="/" component={Home} />
      <Route exact path="/user" component={User} />
    </BrowserRouter>
  )
}

ReactDOM.render(<App />, document.getElementById('root'));
```

#### 稍微高级一下 - 来个 Nested Routing

```jsx
import React from 'react';
import { BrowserRouter, Route, Link } from 'react-router-dom';

function Home() {
  return <div>Home</div>
}

function User() {
  return <div>User</div>
}

const SITE_LIST = ['Alibaba', 'Tencent', 'Apple', 'XiaoMi']

function Site({ match }) {
  const { id } = match.params
  return (
    <div>Site: {id !== '-1' ? SITE_LIST[id] : 'you can choose one first'}</div>
  )
}

export default function App() {
  return (
    <BrowserRouter>
      <nav>
        <ul>
          <li>
            <Link to="/">Home</Link>
          </li>
          <li>
            <Link to="/user">User</Link>
          </li>
          <li>
            <Link to={`/site/-1`}>Site</Link>
            <ul>
              {SITE_LIST.map((s, i) => (
                <li key={i}>
                  <Link to={`/site/${i}`}>{s}</Link>
                </li>
              ))}
            </ul>
          </li>
        </ul>
      </nav>

      <hr />

      <Route exact path="/" component={Home} />
      <Route exact path="/user" component={User} />
      <Route path="/site/:id" component={Site} />
    </BrowserRouter>
  )
}
```

_PS: 有时候用 NavLink 替换 Link 会有惊喜_