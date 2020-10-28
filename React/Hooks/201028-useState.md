## React-Hooks >> useState

---

- React 采用自上而下的单项数据流，管理自身状态与数据
  - 数据只能由父组件触发，向下传递到子组件
  - state 与 props 的改变，都会引起组件的重新渲染，diff 算法
- useState 利用闭包，在函数内部创建一个当前函数组件状态，并提供一个修改该组件状态的方法
- **无论在 class 中还是 hooks 中，state 改变都是 <font color=red>异步</font> 的**
  - 状态异步 意味着：要到下一个事件循环周期执行时，才是最新值

> 思考题

用下面的例子修改状态，会让组件重新渲染吗？

```javascript
const [counter, setCounter] = useState({ a: 1, b: 2 });
counter.b = 4;
setCounter(counter);
```

组件不会重新渲染，但 counter 会改变

> 异步状态所潜藏的危机

```javascript
const [counter, setCounter] = useState(10);
setCounter(20);
console.log(counter); // 此时counter的值，并不是20，而是10
```

> 惯常的接口传参请求

- 两种方案
  - searchByName 会修改属性，不会触发渲染（参考思考题）
  - 使用 useRef 可以 **跨渲染周期保存数据**

```javascript
export default function AsyncDemo() {
  const [param] = useState<Param>({});
  const [listData, setListData] = useState<ListItem[]>([]);

  function fetchListData() {
    // @ts-ignore
    listApi(param).then(res => {
      setListData(res.data);
    })
  }

  function searchByName(name: string) {
    param.name = name;
    fetchListData();
  }

  return [
    <div>data list</div>,
    <button onClick={() => searchByName('Jone')}>search by name</button>
  ]
}
```

> 参见

- [超性感的 React Hooks：useState](https://blog.csdn.net/fedlover/article/details/103347836)
