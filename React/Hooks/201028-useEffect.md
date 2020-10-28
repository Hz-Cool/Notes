## React-Hooks >> useEffect 副作用

---

> useEffect

- useEffect 拥有两个参数 callBack dependencies

  - callBack 回调函数
  - dependencies 依赖数组
    - deps 存在，只有发生变化后 callBack 才会执行。浅比较
    - deps 不存在，callBack 每次 render 都执行

- Q：为什么第二个参数是空数组，相当于 componentMount()
- A：因为 dependdencies 一直不变化，callBack 不会二次执行

- 底层是用数组（链表）实现。
  - 初次渲染：按照 useState,useEffect 顺序，把 state,deps 按照顺序塞入 memoizedState(缓存数组)中
  - 更新：按照顺序，从 memoizedState 中把上次记录值拿出来
- 在 callBack 函数内 return 相当于 componentWillUnmount

> 受控组件

- 组件外部能控制组件内部的状态，则表示该组件为受控组件
  - 利用 props 去改变内部的 state
