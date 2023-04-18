## React-Hooks >> useMemo

---

> ## 一个简单的例子

```javascript
import React, { useState, useMemo } from "react";

function App() {
  const [name, setName] = useState("名称");
  const [content, setContent] = useState("内容");
  return (
    <>
      <button onClick={() => setName(new Date().getTime())}>name</button>
      <button onClick={() => setContent(new Date().getTime())}>content</button>
      <Button name={name}>{content}</Button>
    </>
  );
}

function Button({ name, children }) {
  function changeName(name) {
    console.log("changeName");
    return name + "改变name的方法";
  }
  const otherName = changeName(name);
  return (
    <>
      <div>{otherName}</div>
      <div>{children}</div>
    </>
  );
}

export default App;
```

- 点击父组件按钮时，子组件 name 和 children 会随着发生变化
- 改变 name 和 chuldren 时，changeName 方法都会被调用（做了无用功）

> ## 使用 Memo 优化

- Memo 接收两个参数:
  - 第一个参数为回调函数，返回一个结果
  - 第二个参数为依赖项[数组]，当依赖项中某一个发生变化，结果将会重新计算

```typescript
function useMemo<T>(factory: () => T, deps: DependencyList | undefined): T;
```

- 使用 Memo 能避免无用调用
- 把 const otherName = changeName(name); 替换为：
  - const otherName = useMemo(() => changeName(name), [name])
- 这时改变 content 按钮时并不会调用 changeName，避免了二次渲染

> ## 其他

- Memo 是一个记忆函数，利用闭包缓存上次结果
- 并不会绝对优化，而是一种成本的交换
---
- 闭包是一个特殊的对象
    - 它由两部分组成，执行上下文A以及在A中创建的函数B
    - 当B执行时，如果访问了A中变量对象，那么闭包就会产生


> ## 参见
- [React Hooks ---useMemo](https://segmentfault.com/a/1190000018697490)
- [超性感的React Hooks useCallback、useMemo](https://blog.csdn.net/fedlover/article/details/103347989)