# React-Redux 教程

为了方便使用，Redux 的作者封装了一个 React 的专用库 [React-Redux](https://github.com/reactjs/react-redux),文本主要介绍

![](http://www.ruanyifeng.com/blogimg/asset/2016/bg2016092101.jpg)

<br/><br/>

> ## UI 组件

---

组件分为两大类：

- UI 组件 Presentational Component

  - 只负责 UI 呈现，不带任何业务逻辑
  - 没有状态，不适用 this.state
  - 数据都由 props 提供
  - UI 组件又称 纯组件，纯粹由参数决定
  - ```javascript
    const Title = (value) => <h1>{value}</h1>;
    ```

- 容器组件 Container Component
  - 负责管理数据和业务逻辑，不负责 UI 呈现
  - 带有内部状态

总之：UI 组件负责 UI 呈现，容器组件负责数据和逻辑

<br/><br/>

> ## connerct()

---

React-Redux 提供 `connert` 方法，用于从 UI 组件生成容器组件，将组件连接起来

```javascript
import { connect } from "react-redux";
const VisibleTodoList = connect()(TodoList);
```

`TodoList`是 UI 组件
<br/>
`VisibleTodoList`就是 React-Redux 通过 connect 方法生成的容器组件

因为没有业务逻辑，上面这个容器组件只是 UI 组件的一个单纯包装层
<br/>
为了定义业务逻辑，需要给出下面两方的信息

- 输入逻辑：外部数据(state 对象)如何转换为 UI 组件参数
- 输出逻辑：用户发出动作如何变为 Action 对象，从 UI 组件传出去

```javascript
import { connect } from "react-redux";

const VisibleTodoList = connect(mapStateToProps, mapDispatchToProps)(TodoList);
```

`connerct`方法接受两个参数：`mapStateTpProps` 和 `mapDispatchToProps`
<br/>
前者负责输入逻辑，即将`state`映射到 UI 组件的参数(props)
<br/>
后者负责输出逻辑，即将用户对 UI 组件的操作映射成 Action

<br/><br/>

> ## mapStateToProps

---

`mapStateToProps` 是一个函数，建立一个从(外部的)`state`对象到(UI 组件的)`props`对象的映射关系
<br/>
`mapStateToProps`执行后返回一个对象，里面的每一个键值对就是一个映射

```javascript
const mapStateToProps = (state) => {
  return {
    todos: getVisibleTodos(state.todos, state.visibilityFilter),
  };
};
```

`mapStateToProps`是一个函数，接收 state 作为参数
<br/>
返回一个对象,属性 `todus` 表示 UI 组件的同名参数
<br/>
`getVisibleTodos`是一个函数，可以从 `state` 计算出 `todos` 的值

```javascript
const getVisibleTodos = (todos, filter) => {
  switch (filter) {
    case "SHOW_ALL":
      return todos;
    case "SHOW_COMPLETED":
      return todos.filter((t) => t.completed);
    case "SHOW_ACTIVE":
      return todos.filter((t) => !t.completed);
    default:
      throw new Error("Unknown filter: " + filter);
  }
};
```

`mapStateToProps` 会订阅 Store，每当 `state` 更新会自动执行，重新计算 UI 组件参数，从而触发 UI 组件的重新渲染

`mapStateToProps` 的第一个参数总是 `state`对象，还有第二个参数，代表容器组件的 props 对象

```javascript
const mapStateToProps = (state, ownProps) => {
  return {
    active: ownProps.filter === state.visibilityFilter,
  };
};
```

使用 `ownProps` 作为参数后，如果容器组件的参数发生变化，也会引发 UI 组件重新渲染

`connerct` 方法何以省略 `mapStateToProps` 参数，UI 组件就不会订阅 Store，Store 更新不会引起 UI 组件更新

<br/><br/>

> ## mapDispatchToProps

---

`mapDispatchToProps` 是 `connect` 的第二个参数，用来建立 UI 组件的参数到 `store.dispatch` 方法的映射
<br/>
它定义了用户的操作应该当作 Action 传给 Store,它可以是一个函数，也可以是一个对象

如果 `mapDispatchToProps` 是一个函数，会得到 `dispatch` 和 `ownProps`(容器组件的 props 对象)两个参数

```javascript
function mapDispatchToProps(dispatch) {
  return {
    dispatch,
  };
}
```

`mapDispatchToProps` 作为函数，返回一个对象，定义了 UI 组件的参数怎样发出 Action

<br/><br/>

> ## Provider 组件

---

`connerct` 方法生成容器组件后，需要让容器组件拿到 `state` 对象，才能生成 UI 组件的参数

React-Redux 提供 `Provider` 组件，可以让容器组件拿到 `state`

```javascript
import { Provider } from "react-redux";
import { createStore } from "redux";
import todoApp from "./reducers";
import App from "./components/App";

let store = createStore(todoApp);

render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById("root")
);
```

`Provider` 在根组件外面包了一层， `APP`的所有组件就默认可以拿到 state 了

原理是 React 组件的属性

```javascript
class Provider extends Component {
  getChildContext() {
    return {
      store: this.props.store,
    };
  }
  render() {
    return this.props.children;
  }
}
```

<br/><br/>

> ## 最后

---

正常开发很少会直接使用 Redux ，而是配合其他库一起使用

- [@rematch/core](https://www.npmjs.com/package/@rematch/core)
  <br/>
  rematch 是对 redux 的二次封装，简化了 redux 是使用，极大的提高了开发体验。

- [dvajs](https://dvajs.com/)

<br/><br/>

> ## 参见

---

- [阮一峰-React-Redux 的用法](http://www.ruanyifeng.com/blog/2016/09/redux_tutorial_part_three_react-redux.html)
- [Rematch: Redux 的重新设计](https://segmentfault.com/a/1190000019056045)
- [rematch 的基本用法](https://www.cnblogs.com/mengff/p/9510264.html)
