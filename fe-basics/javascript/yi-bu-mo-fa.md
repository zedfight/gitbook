# 异步魔法

![JavaScript异步魔法.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/29b4b6e608fe470daaf6944bd52b4ba6\~tplv-k3u1fbpfcp-jj-mark:0:0:0:0:q75.image#?w=1208\&h=680\&s=330546\&e=png\&b=d4defe) JavaScript 是一门**单线程**的编程语言，意味着**同一时间只能执行一个任务**。然而，现代的 Web 应用程序通常需要处理大量的异步操作，如网络请求、文件读写、用户输入等。如果这些所有操作都是同步的，会导致应用程序在等待某些操作完成时被阻塞，用户体验将变得非常差。

为了更好地处理这些异步操作，JavaScript 引入了异步编程的概念。在同步编程中，代码会按照顺序执行，每个操作都要等待前一个操作完成才能继续。相比之下，异步编程的代码不是按顺序执行的，允许程序在执行某个操作时不必等待该任务完成，而是可以继续执行后续的代码。异步编程在处理**非阻塞 I/O 操作**、**事件驱动**等方面发挥了重要作用，为**提高性能**、**避免阻塞**和提供更好的用户体验等方面带来了优势。

## 回调函数（Callback）

在 JavaScript 中，最早的异步编程方式是通过回调函数。当需要执行一个耗时的操作时，比如发起一个网络请求或读取文件，可以通过将一个函数作为参数传递给异步操作，并在操作完成后调用该函数，实现异步回调。

随着应用程序变得更为复杂，多个异步操作的嵌套使用会导致回调函数的嵌套，形成所谓的**回调地狱（Callback Hell）**。

```javascript
getData(function(result1) {
  processData(result1, function(result2) {
    processMoreData(result2, function(result3) {
      // ... 还有更多嵌套的回调
    });
  });
});
```

这种嵌套结构使代码高度耦合，难以理解和维护，容易引发错误。在回调函数中，不仅难以清晰地捕捉和处理错误，且无法使用`return`语句。为了克服回调地狱的问题，涌现了一系列解决方案。

## 观察者模式

`观察者模式`定义了对象间**一对多**的依赖关系，当一个对象的状态发生改变时，所有依赖于它的对象都会得到通知并自动更新。在 JavaScript 中，**事件监听是观察者模式的一种实现方式**。

事件监听是一种常见的异步编程方式，特别适用于基于事件触发的场景，如浏览器的 DOM 操作和 NodeJS 的事件驱动。通过事件监听，程序可以注册对特定事件的监听器，在事件发生时执行相应的回调函数。

```javascript
// 在浏览器中
// 事件监听
document.getElementById('myButton').addEventListener('click', function() {
  console.log('Button clicked!');
});

// 触发事件
document.getElementById('myButton').click(); // 模拟按钮点击事件

// 在 Node.js 中
const EventEmitter = require('events');

// 主题
class Subject extends EventEmitter {
  setState(state) {
    this.emit('stateChange', state);
  }
}

// 观察者
class Observer {
  update(state) {
    console.log('Received updated state:', state);
  }
}

const subject = new Subject();
const observer = new Observer();

// 添加观察者到主题
subject.on('stateChange', observer.update.bind(observer));

subject.setState('newState');
```

**RxJS** 也是一个基于观察者模式的库，它通过使用可观察对象（Observables）来处理异步和事件驱动的编程。具体使用可参考[RxJS文档](https://rxjs.dev/guide/overview)。

观察者模式使得代码更具**模块化**和**解耦性**。然而，需要注意整个程序变成事件驱动模型，且事件监听器的注册和触发被分离，可能导致**运行流程不够清晰**。在使用观察者模式时，需权衡使用场景，以确保代码结构清晰且易于理解。

## 发布订阅模式

发布-订阅是一种**消息范式**，它也是一种处理异步编程的方式，类似于观察者模式，但有一个中间的事件通道，通常被称为**事件总线**。消息的发送者不会将消息直接发送给特定的接收者。而是将发布的消息分为不同的类别，无需了解哪些订阅者可能存在。同样的，订阅者可以表达对一个或多个类别的兴趣，只接收感兴趣的消息，无需了解哪些发布者存在。

相对于事件监听，发布-订阅模式**耦合度更低**，允许发布者和订阅者之间进行解耦。在 Node.js 中，`EventEmitter` 可以同时实现观察者模式和发布订阅模式的特性，具体取决于使用方式。

以下是发布-订阅模式的简单实现：

```javascript
const EventEmitter = require('events');

class EventBus extends EventEmitter {}

const eventBus = new EventBus();

// 订阅者
function subscriber(state) {
  console.log('Received updated state:', state);
}

// 添加订阅者到事件总线
eventBus.on('stateChange', subscriber);

// 发布者
function publisher(newState) {
  eventBus.emit('stateChange', newState);
}

// 触发状态变化
publisher('newState');
```

发布订阅模式和观察者模式都是解决回调地狱问题的有效手段。发布订阅模式以其灵活性和松散耦合的特点，更适用于复杂的系统，而观察者模式则更为简单，适用于规模较小的场景。

然而，在处理多个观察者或订阅者时，这两种模式可能会**使代码变得复杂**，**增加维护和理解的成本**。因此，在选择模式时，需要根据项目规模和复杂性权衡各种因素，以确保代码的清晰性和可维护性。

## Promise

`Promise` 是 ES6 中新增的一种异步编程解决方案，它是对回调函数的一种补充，可以避免回调地狱问题。Promise 本质上是一个容器，里面保存着某个未来才会结束的事件（通常是一个异步操作）的结果。

一个 Promise 对象包含其原型，状态值（pending/fulfilled/rejected），值（then 方法返回的值）。Promise 对象创建后将立即执行，其中的函数执行会阻塞，而函数参数 `resolve` 和 `reject` 的执行时则会挂到微任务中执行。

状态值：

**Pending（进行中）**：表示异步操作尚未完成，仍在进行中。

**Fulfilled（已完成）**：表示异步操作成功完成，可以通过 then 方法获取结果。

**Rejected（已失败）**：表示异步操作失败，可以通过 catch 方法或另一个then中的第二个参数捕捉错误。

核心方法：

**resolve**：将 Promise 状态从 pending 转变为 fulfilled，并传递结果值。

**reject**：将 Promise 状态从 pending 转变为 rejected，并传递错误信息。

创建 Promise 对象的基本语法如下：

```javascript
const promise = new Promise((resolve, reject) => {
  // 异步操作，只有执行 resolve/reject 才可以决定 Promise 状态，任何其他操作都无法改变这个状态。一旦状态改变，就不会再变。
  if (/* 异步操作成功 */) {
    resolve(result); // 将结果传递给 then
  } else {
    reject(error); // 将结果传递给 then 的第二个参数或者 catch 捕获
  }
});
```

Promise 的**链式调用**通过 then 方法实现，可以更清晰地表达异步操作的顺序和逻辑：

```javascript
promise
  .then(result => {
    // 处理成功的结果
    return result;
  })
  .then(result2 => {
    // 处理result2
    return result2;
  })
  .then(result3 => {
    // 处理result3
    return result3
  })
  .catch(error => {
    // 处理错误
  });
```

在Promise中，`then` 方法的第一个参数回调函数用于处理异步操作成功的结果，而 `then` 方法的第二个参数回调或 `catch` 方法则用于处理异步操作失败的情况。

Promises 广泛应用于处理 JavaScript 中的异步操作，包括 AJAX 请求、定时任务、文件操作等。尽管 Promise 解决了回调地狱等问题，但也有一些**缺点**：

其一，无法取消 Promise 中的 fn，一旦新建它就会立即执行，无法中途取消。

其二，如果不设置回调函数，Promise 内部抛出的错误，不会反应到外部。

其三，当处于 pending 状态时，无法得知目前进展到哪一个阶段（刚刚开始还是即将完成）。

其四，代码冗余，原来的任务被 Promise 包装了一下，不管什么操作，一眼看去都是一堆 then，语义变得不清晰。

## Generator 函数

ES6 引入的 `Generator 函数`是一种强大的异步编程解决方案，实际上是协程的一种实现。作为状态机，Generator 函数封装了多个内部状态，通过使用 `yield` 和 `return` 表达式实现了执行的暂停和恢复。执行 Generator 函数返回一个遍历器对象，通过该对象的 `next`、`return` 和 `throw` 方法，可以逐步遍历 Generator 函数内部的每个状态。这一特性使得异步编程更加直观和可控。

**yield 表达式**： 标记暂停执行的位置，只有当调用 next 方法、内部指针指向该语句时才会执行，为 JavaScript 提供了手动的“惰性求值”功能。

```javascript
function* gen() {
  yield 123 + 456;
}

const generatorInstance = gen();

// 惰性求值，只在需要时触发计算
console.log(generatorInstance.next().value);  // 输出: 579
```

**return 表达式**： 标记 Generator 函数的结束位置，调用 return 方法后，Generator 函数执行会终止，并且后续的逻辑将不再执行。

```javascript
function* myGenerator() {
  yield 1;
  return 2; // 执行到return时，生成器函数终止
  yield 3; // 这个语句不会执行
}

const generator = myGenerator();

console.log(generator.next()); // 输出: { value: 1, done: false }
console.log(generator.next()); // 输出: { value: 2, done: false }
console.log(generator.next()); // 输出: { value: undefined, done: true }
```

**Generator.prototype.next()**：用于恢复 Generator 函数的执行，可以带一个参数，该参数会被当作上一个 yield 表达式的返回值。Generator 函数从暂停状态到恢复运行，它的上下文状态（context）是不变的。通过next方法的参数，就有办法在 Generator 函数开始运行之后，继续向函数体内部注入值。也就是说，可以在 Generator 函数运行的不同阶段，从外部向内部注入不同的值，从而调整函数行为。

```javascript
function* f() {
  for(var i = 0; true; i++) {
    var reset = yield i;
    if(reset) { i = -1; }
  }
}

var g = f();

g.next() // { value: 0, done: false }
g.next() // { value: 1, done: false }
g.next(true) // { value: 0, done: false }
```

**Generator.prototype.return()**：返回给定的值，并终结遍历 Generator 函数。

```javascript
function* gen() {
  yield 1;
  yield 2;
  yield 3;
}

var g = gen();

g.next()        // { value: 1, done: false }
g.return('foo') // { value: "foo", done: true }
g.next()        // { value: undefined, done: true }
```

遍历器对象 g 调用 return() 方法后，返回值的 value 属性就是 return() 方法的参数 foo。并且，Generator 函数的遍历就终止了。如果 return() 方法调用时，不提供参数，则返回值的 value 属性为 undefined。

**Generator.prototype.throw()**：可以在函数体外抛出错误，然后在 Generator 函数体内捕获。

```javascript
var g = function* () {
  try {
    yield;
  } catch (e) {
    console.log('内部捕获', e);
  }
};

var i = g();
i.next();

try {
  i.throw('a');
  i.throw('b');
} catch (e) {
  console.log('外部捕获', e);
}
// 内部捕获 a
// 外部捕获 b
```

遍历器对象 i 连续抛出两个错误。第一个错误被 Generator 函数体内的 catch 语句捕获。i 第二次抛出错误，由于 Generator 函数内部的 catch 语句已经执行过了，不会再捕捉到这个错误了，所以这个错误就被抛出了 Generator 函数体，被函数体外的 catch 语句捕获。

**yield\* 表达式**：如果在 Generator 函数内部，调用另一个 Generator 函数。需要在前者的函数体内部，自己手动完成遍历。

```javascript
function* foo() {
  yield 'a';
  yield 'b';
}

function* bar() {
  yield 'x';
  // 手动遍历 foo()
  for (let i of foo()) {
    yield i;
  }
  yield 'y';
} 

// 等价于
function* bar() {
  yield 'x';
  yield 'a';
  yield 'b';
  yield 'y';
} 

for (let v of bar()){
  console.log(v);
}
// x a b y
```

手动遍历写法非常麻烦，es6 提供了 `yield*` 表达式，用来在一个 `Generator` 函数里面执行另一个 `Generator` 函数。

```javascript
function* foo() {
  yield 'a';
  yield 'b';
}

function* bar() {
  yield 'x';
  yield* foo();
  yield 'y';
}

for (let v of bar()){
  console.log(v);
}
// x a b y
```

`Generator` 函数具有广泛的应用场景，包括但不限于：

**迭代器（Iterator）**：生成器函数可以用于创建自定义迭代器。通过使用 yield 在每次迭代中生成下一个值，可以方便地实现自定义迭代逻辑。

**异步操作**：生成器函数可以与 yield 结合使用，实现更清晰的异步代码。

\*\* 协程（Coroutine）\*\*：生成器函数可以模拟简单的协程，允许在函数执行过程中暂停和恢复。

**惰性计算**：生成器函数可以用于处理大量的数据流，逐步生成和处理数据，而不需要一次性加载所有数据，优化性能。

**流式处理**：多步操作非常耗时，使用 Generator 按次序自动执行所有步骤。

**处理文件内容**：当处理大型文件时，Generator 可以逐行读取文件内容，而不必一次性读取整个文件到内存。

**节省资源**：在一些情况下，如果生成的数据只在迭代过程中使用，而不需要保存在内存中，使用 Generator 可以节省系统资源。

虽然 Generator 避免了回调地狱和 Promise 链式调用的复杂性，但 Generator 会使得代码更加复杂和难以理解，尤其对于初学者而言可能需要花费更多时间来适应和理解这种编程模型。

## Async/Await

ES2017 引入了 `Async/Await` 语法糖，为异步编程带来了更直观、更易读的方式。Async/Await 是 Generator 和 Promise 的综合体，Async/Await 中还自带`自动执行器`，且 `await` 后跟的是 `Promise`。其语法糖特性让异步代码看起来更像同步代码，消除了回调地狱问题，提高了可读性和可维护性。

基本结构如下：

```javascript
async function myAsyncFunction() {
  // 异步操作
  const result = await someAsyncOperation();
  return result;
}
```

`async` 函数使用关键字 `async` 声明，内部使用 `await` 关键字等待异步操作完成。在 `async` 函数中，`await` 暂停函数的执行，等待 `Promise` 解决并返回。

错误处理方面，async 函数采用传统的 `try-catch` 结构，使代码结构更加清晰：

```javascript
async function fetchData() {
  try {
    const result1 = await getData();
    const result2 = await processData(result1);
    const result3 = await processMoreData(result2);
    // ... 后续操作
    return finalResult;
  } catch (error) {
    // 处理错误
  }
}
```

需要注意的是，async 函数返回的 Promise 对象会在内部所有 await 命令后面的 Promise 对象执行完毕之后才会发生状态改变，除非在此过程中遇到 return 语句或发生错误。

总体而言，async 函数的实现最简洁、最符合语义，几乎没有语义不相关的代码。

## 总结

这些不同的异步编程方式各有优劣，选择合适的方式取决于项目的需求、复杂性和开发团队的经验。在实际应用中，可以根据具体情况灵活选择异步编程的方式，以提高代码的质量和可维护性。
