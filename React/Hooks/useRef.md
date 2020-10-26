## React-Hooks >> useMemo

---

> ## 介绍

- 函数式组件中 useRef 是一个返回可变引用对象的函数

  - 该对象 **.current** 属性的初始值为 useRef 传入的参数 **initalVale**
  - 返回的对象在组件整个生命周期中持续存在

  ```javascript
  const ref = useRef(initialValue);
  ```

- useRef 的两种用途
  - 访问 DOM 节点,或者 React 元素
  - **跨渲染周期**保存数据

> ## 访问 DOM 节点

```javascript
import React, { useState, useEffect, useMemo, useRef } from "react";

export default function App(props) {
  const [count, setCount] = useState(0);

  const doubleCount = useMemo(() => {
    return 2 * count;
  }, [count]);

  const couterRef = useRef();

  useEffect(() => {
    document.title = `The value is ${count}`;
    console.log(couterRef.current);
  }, [count]);

  return (
    <>
      <button
        ref={couterRef}
        onClick={() => {
          setCount(count + 1);
        }}
      >
        Count: {count}, double: {doubleCount}
      </button>
    </>
  );
}
```

- useRef 创建了 couterRef 对象,并赋值给了 Button 的 ref 属性
- 可通过 couterRef.current 就可访问到 Button 的 DOM 对象

> ## 保存数据

- state 可以在多次渲染后依旧保持不变
  - 但是 state 一旦修改会造成组件重新渲染
- useRef 可以跨渲染周期存储数据,对其修改也不会引起组件渲染

```javascript
import React, { useState, useEffect, useMemo, useRef } from "react";

export default function App(props) {
  const [count, setCount] = useState(0);

  const doubleCount = useMemo(() => {
    return 2 * count;
  }, [count]);

  const timerID = useRef();

  useEffect(() => {
    timerID.current = setInterval(() => {
      setCount((count) => count + 1);
    }, 1000);
  }, []);

  useEffect(() => {
    if (count > 10) {
      clearInterval(timerID.current);
    }
  });

  return (
    <>
      <button
        onClick={() => {
          setCount(count + 1);
        }}
      >
        Count: {count}, double: {doubleCount}
      </button>
    </>
  );
}
```

> ## 参见

- [React—useRef](https://blog.csdn.net/hjc256/article/details/102587037)
- [超性感的 React Hooks useRef](https://blog.csdn.net/fedlover/article/details/103347975)
