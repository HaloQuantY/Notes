## ES6 Generator和Async函数

### Generator函数

​	是ES6提供的一种异步编程方案。Generator函数内部封装了多个状态，而返回一个可以遍历内部状态的遍历对象，是一个遍历对象生成函数。

​	Generator函数的特征是在function后会紧跟一个型号*，其次函数体内部会使用yield表达式来定义不同的状态。

```javascript
function* helloWorldGenerator() {
    // 内部三个状态，最后一个状态用return表示执行结束
  yield 'hello';
  yield 'world';
  return 'ending';
}
// 调用Generator函数后并不会执行，而是返回一个指向内部状态指针
var hw = helloWorldGenerator();
```

​	

​	访问内部状态用遍历对象的next方法

```javascript
hw.next()
// { value: 'hello', done: false }

hw.next()
// { value: 'world', done: false }

hw.next()
// { value: 'ending', done: true }

hw.next()
// { value: undefined, done: true }
```

​	每次调用next函数也会返回一个对象，对象的value属性是yield后的表达式结果。