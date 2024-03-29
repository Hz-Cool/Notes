# 考察 async await 的掌握情况

> 代码的运行结果是什么？

```javascript
const Err = async () => {
	throw new Error(42);
};

const Obj = {
	async A (){
		try {
			return Err();
		} catch {
			console.log('A');
		}
	},
	async B (){
		try {
			await Err();
		} catch {
			console.log('B');
		}
	},
	async C (){
		try {
			Err();
		} catch {
			console.log('C');
		}
	},
};

( async () => {
	for( const key in Obj )
	{
		try {
			await Obj[key]();
		} catch {
			console.log('D');
		}
	}
} )();
```

<br><br>

> 答案

这个掌握两个原理就行：

1.async 函数返回 Promise 
<br>
2.Promise 只有被取值才能捕捉到错误，所以要么 Promise.prototype.catch()，要么就用 await+try/catch 。

A 部分 Err()返回的是个 Promise，所以 try 捕获不到错误，return 出去之后在主函数里 await，就取到了这个错误，被主函数的 try 捕捉到，输出 D 。

B 部分 Err()返回 Promise，被 await 取到错误，被 B 的 try 捕捉到，输出 B 。

C 部分 Err()返回 Promise，C 的 try 捕捉不到错误，因为 C()返回的是一个 undefined，所以主函数的 try 也捕捉不到这个错误，于是这一次循环什么都没输出。
但是可能 JS 引擎会提示有一个 Uncaught 的异常，这个得看 JS 引擎的策略，有的引擎不提示，提示的话如果还输出 message 的话就会输出 42 。

<br><br>
> 其他知识点

`for in object` 是否有序

关于顺序可以参考 ECMA-262 2022 的这个规范： <br>
https://tc39.es/ecma262/#sec-ordinaryownpropertykeys

我看 ES6 的时候就是这样了，只不过章节名有变： <br>
https://262.ecma-international.org/6.0/#sec-ordinary-object-internal-methods-and-internal-slots-ownpropertykeys
<br>
ES5.1 内容变化太大，没工夫看，有人看过可以发一下情况。


翻译过来大义就是:
<br>
1.对象的属性如果可以被当做“array index”(如是数字或只包含数字的字符串)的话，就按照索引数值升序排序；
<br>
2.其他情况按照属性的创建时间升序排序。

JS 的解析是按照代码自上而下的，所以题主给的这个代码的顺序是固定的，不会在执行中变化。

<br><br>

> 引用

[如果你自认熟悉 Promise，来猜一下这个代码的运行结果](https://v2ex.com/t/789253)
