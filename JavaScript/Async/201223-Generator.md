> ## Generator 函数

Generator 是 JS 模拟`协程`在 ES6 的实现，最大特点是可以交出函数的执行权（暂停执行）

```javascript
function* gen(x) {
  var y = yield x + 2;
  return y;
}
var g = gen(1);
g.next(); // { value: 3, done: false }
g.next(); // { value: undefined, done: true }
```

- 整个 Generator 函数是一个封装的异步任务容器
  - 异步需要暂停的地方使用 yield 语句注明
  - 函数名之前加 \* 是和普通函数的区别
- 调用 Generator 函数，会返回一个内部指针(遍历器) g
  - Generator 区别于普通函数的另一个地方是执行它不会返回结果，返回的是指针对象
  - 调用指针 g 的 next 方法，会移动内部指针指向第一个遇到 yield 语句（上例是执行到 x+2）
  - 每次调用 next 方法，会返回一个对象，表示当前阶段的信息(value,done)
    - value 属性是 yield 语句后面表达式的值，表示当前阶段的值
    - done 属性是一个布尔值，表示 Generator 函数是否执行完毕，是否还有下一个阶段

> ### Generator 函数的数据交换和错误处理

- Generator 能够暂停和恢复执行，这是它能封装异步任务的根本原因。
- 除此之外它还有个特性：函数体外的数据交换和错误处理机制
  - next 方法返回值的 value 属性是 Generator 函数向外输出数据
  - next 方法还能接收参数，向内输入数据

```javascript
function* gen(x) {
  var y = yield x + 2;
  return y;
}

var g = gen(1);
g.next(); // { value: 3, done: false }
g.next(2); // { value: 2, done: true }
```

- 第一个 next 方法的 value 属性，返回表达式 x+2 的值为(3)
- 第二个 next 方法带有参数 2，这个参数作为上个阶段异步任务的结果被变量 y 接收,因此 value 的属性值返回的就是 2 (变量 y 的值)

<br/><br/>
Generator 可以部署错误处理，捕捉函数体外抛出的错误

```javascript
function* gen(x) {
  try {
    var y = yield x + 2;
  } catch (e) {
    console.log(e);
  }
  return y;
}

var g = gen(1);
g.next();
g.throw("出错了");
// 出错了
```

- 在 generator 函数体外，使用指针对象的 throw 方法抛出错误，可以被函数体内的 try catch 代码块捕获。
- 这意味着，出错的代码与处理错误的代码，实现了时间和空间上的分离

  <br/> <br/>

> ### 线程和携程的区别

- 实现方式

  - 线程通常是系统实现的（也有语言模拟的，如 Python 的线程）
  - 携程通常是库实现的（也有语言实现的，如 Go）

- 控制权

  - 线程控制权通常是系统控制，是被动的
  - 携程的控制通常是调用方主动控制的（需要 yield 释放控制权）

- 态的切换

  - 由于线程是系统控制，通常切换需要进入内核态进行处理
  - 携程通常是在用户态就完成了切换
  - 携程切换是在一个线程内进行的

<br/><br/>
