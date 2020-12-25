> ## Event Loop

![](https://mmbiz.qpic.cn/mmbiz_gif/0icXZFonEWX5MgF7OZ4veUBBCIsujXB5VNQcgibicrzibDpYDqYvyIe7qK3ez4y9icdVjnhgz76FVjzkyWLGpG5QozA/640)

Event Loop 是一个执行模型，浏览器和 NodeJS 基于不同的技术实现了各自的 Event Loop

- 浏览器的 Event Loop 再 H5 中明确定义
- NodeJS 的 Event Loop 是基于 libuv 实现,可参考[node 官方文档](https://nodejs.org/en/docs/guides/event-loop-timers-and-nexttick/)

<br/><br/>

> ## 宏队列和微队列

`宏队列 mcacrotask` 也叫 tasks。一些异步任务的回调会依次进入 macro task queue,等待后续被调用

- setTimeout
- setInterval
- setImmediate(Node 独有)
- requestAnimationFrame(浏览器独有)
- I/O
- UI rendering(浏览器独有)

<br/>

`微队列 microtask` 也叫 jobs。另一些异步任务的回调以此进入 micro task queue,等待后续被调用

- process.nextTick(Node 独有)
- Promise
- Object.observe
- Mutatineobserve

> ## 浏览器的 Event Loop

![](https://image-static.segmentfault.com/127/150/1271505701-5a659863bb046_articlex)

执行 Javascript 的具体流程

1. 执行全局 Script 同步代码，这些同步代码有一些是同步语句，有一些是异步语句
2. 全局 Script 代码执行完毕后，调用栈 Stack 会清空
3. 从`微队列(microtask queue)`列去除位于队首的任务，放入调用栈 Stack 中执行，执行完毕后`微队列`长度减 1
4. 如果在执行`微队列`过程中，又产生了新任务，则该任务放入队列的队尾。
5. `微队列`中所有任务都执行完毕，此时`微队列`为空，调用栈 Stack 也为空
6. 取出`宏队列(macrotask queue)`中位于队首的任务，放入 Stack 中执行
7. 执行完成后，调用栈 Stack 为空
8. 重复 3 ~ 7 步骤

三个重点

- **`宏队列` 一次只从队列中取出一个任务执行，执行完成后就执行 `微队列` 队列中的任务;**
- `微队列`中所有任务都会被取出来，直到队列为空
- 上图没有 rendering 节点，在执行完`微队列`之后,下一个`宏队列`之前,紧跟执行 UI render

> ## 习题练习

```javascript
console.log(1);

setTimeout(() => {
  console.log(2);
  Promise.resolve().then(() => {
    console.log(3);
  });
});

new Promise((resolve, reject) => {
  console.log(4);
  resolve(5);
}).then((data) => {
  console.log(data);
});

setTimeout(() => {
  console.log(6);
});

console.log(7);
```

执行过程

> **Step 1**

```
  console.log(1)
```

- Stack Queue : [console]
- Macrotask Queue: []
- Microtask Queue: []
- 打印结果 `1`

---

> **Step 2**

```javascript
setTimeout(() => {
  // 这个回调函数叫做callback1，setTimeout属于macrotask，所以放到macrotask queue中
  console.log(2);
  Promise.resolve().then(() => {
    console.log(3);
  });
});
```

- Stack Queue: [setTimeout]
- Macrotask Queue: [callback1]
- Microtask Queue: []
- 打印结果 `1`

---

> **Step 3**

```javascript
new Promise((resolve, reject) => {
  // 注意，这里是同步执行的，如果不太清楚，可以去看一下promise的实现
  console.log(4);
  resolve(5);
}).then((data) => {
  // 这个回调函数叫做callback2，promise属于microtask，所以放到microtask queue中
  console.log(data);
});
```

- Stack Queue: [promise]
- Macrotask Queue: [callback1]
- Microtask Queue: [callback2]
- 打印结果 `1 4`

---

> **Step 4**

```javascript
setTimeout(() => {
  // 这个回调函数叫做callback3，setTimeout属于macrotask，所以放到macrotask queue中
  console.log(6);
});
```

- Stack Queue: [setTimeout]
- Macrotask Queue: [callback1, callback3]
- Microtask Queue: [callback2]
- 打印结果 `1 4`

---

> **Step 5**

```javascript
console.log(7);
```

- Stack Queue: [console]
- Macrotask Queue: [callback1, callback3]
- Microtask Queue: [callback2]
- 打印结果 `1 4 7`

---

> > 全局的 Script 代码已执行完毕，进入下一个步骤，从`微队列`中依次取出任务执行,直到`微队列`为空

---

> **Step 6**

```javascript
console.log(data); // 这里data是Promise的决议值5
```

- Stack Queue: [callback2]
- Macrotask Queue: [callback1, callback3]
- Microtask Queue: []
- 打印结果 `1 4 7 5`

---

> > `微队列`中所有任务执行完毕，开始从`宏队列`中执行队首任务

---

> **Step 7**

```javascript
console.log(2);
```

- Stack Queue: [callback1]
- Macrotask Queue: [callback3]
- Microtask Queue: []
- 打印结果 `1 4 7 5 2`

---

> > 执行 callback1 时遇到了另一个 Promise,Promise 执行完后在微队列中注册了一个 callback4 回调函数

---

> **Step 8**

```javascript
Promise.resolve().then(() => {
  // 这个回调函数叫做callback4，promise属于微队列
  console.log(3);
});
```

- Stack Queue: [promise]
- Macrotask : [callback3]
- Microtask Queue: [callback4]
- 打印结果 `1 4 7 5 2`

---

> > 执行完一个宏任务后，会再去执行微队列中的所有任务

---

> **Step 9**

```javascript
console.log(3);
```

- Stack Queue: [callback4]
- Macrotask Queue: [callback3]
- Microtask Queue: []
- 打印结果 `1 4 7 5 2 3`

---

> > 微队列执行完成后，会再去执行队首的宏任务

---

> **Step 10**

```javascript
console.log(6);
```

- Stack Queue: [callback3]
- Macrotask Queue: []
- Microtask Queue: []
- 打印结果 `1 4 7 5 2 3 6`

---

至此所有队列都已清空，最终打印结果为 `1 4 7 5 2 3 6`

再来一个例子

```javascript
console.log(1);

setTimeout(() => {
  console.log(2);
  Promise.resolve().then(() => {
    console.log(3);
  });
});

new Promise((resolve, reject) => {
  console.log(4);
  resolve(5);
}).then((data) => {
  console.log(data);

  Promise.resolve()
    .then(() => {
      console.log(6);
    })
    .then(() => {
      console.log(7);

      setTimeout(() => {
        console.log(8);
      }, 0);
    });
});

setTimeout(() => {
  console.log(9);
});

console.log(10);
```

答案是：

```
1 4 10 5 6 7 2 3 9 8
```

> > > 加载过程可视化

![](https://mmbiz.qpic.cn/mmbiz_gif/0icXZFonEWX5MgF7OZ4veUBBCIsujXB5VEpxS4ajcuJRGaYOiaJSpicFf1ib7jOHRYco9UESDG1ic5CaqSViacKxZfPg/640)

![](https://mmbiz.qpic.cn/mmbiz_gif/0icXZFonEWX5MgF7OZ4veUBBCIsujXB5V6Jgr7RD1XI0PtnhrIJJbRSicXNGWB91pqJSygY2HasHe5QOHdB8h8DA/640)

![](https://mmbiz.qpic.cn/mmbiz_gif/0icXZFonEWX5MgF7OZ4veUBBCIsujXB5Vjv9WsADGroicAJKVUvBVo7Oqhv3rDJhDmDRGK16XHZ1637pIHw98f2A/640)

![](https://mmbiz.qpic.cn/mmbiz_gif/0icXZFonEWX5MgF7OZ4veUBBCIsujXB5VusC2kc0fUu4u1ribnaUiaq8PksswOxLo3qWicq7ks2yn4bRWoCwC9Ll9w/640)

![](https://mmbiz.qpic.cn/mmbiz_gif/0icXZFonEWX5MgF7OZ4veUBBCIsujXB5VaIBCiaTsSwoekh2vHlP0H0VCDlibT0QO26L5EibuDzEVQCbo4ew1icbLBQ/640)

> ## 参见

- [带你彻底弄懂 Event Loop](https://segmentfault.com/a/1190000016278115)

- [JavaScript 可视化](https://mp.weixin.qq.com/s/O1prXsE05ythj5hTM4Op6A)
