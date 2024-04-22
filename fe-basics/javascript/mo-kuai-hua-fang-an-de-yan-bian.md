# 模块化方案的演变

<figure><img src="../.gitbook/assets/modular-solution.png" alt=""><figcaption></figcaption></figure>

在 JavaScript 的演进过程中，长期缺乏模块化方案，这导致开发者难以将大型项目分解为互相依赖的小模块，进而组装起来。这一挑战妨碍了庞大而复杂项目的开发。模块化方案的出现填补了这一空白，它们将`一个模块定义为一个独立的文件，具有自己的作用域，并且只暴露指定的变量和函数给外部`。

模块化方案的优势在于提高了代码的**可复用性**、**可拓展性**和**可维护性**，从而提升了开发效率和灵活性。随着 JavaScript 的发展，出现了多种模块化方案，如 CommonJS 和 ES6 模块等。每种方案都有其独特的特点和适用场景，开发者可以根据项目的需求选择最合适的模块化方案。

### CommonJS

CommonJS 是一种**服务器端模块化规范**，最初专为 Node.js 设计。它采用**同步加载模块**的方式，在**运行时加载整个模块**并生成一个对象。然后从该对象上读取导出的方法或变量。这种同步加载方式存在**阻塞**，因为只有在加载完成后才能执行后续操作。

Node.js 主要用于服务器编程的场景中，模块文件通常存在于本地硬盘缓存，因此读取速度较快，同步加载在此环境中不会引起问题。CommonJS 的写法通过使用 `require` 引入模块，而 `module.exports` 用于导出模块。

以下是CommonJS的示例代码：

```typescript
let { stat, exists, readfile } = require('fs');

// 与上述写法等效
let _fs = require('fs');
let stat = _fs.stat;
let exists = _fs.exists;
let readfile = _fs.readfile;
```

CommonJS 模块在加载过程中会维护一个**缓存数据结构**，其中包含模块的ID、导出对象等信息。这有助于提高加载效率，避免重复加载相同的模块。

```json
{
    id: '...',
    exports: { ... },
    loaded: true,
    ...
}
```

### AMD (Asynchronous Module Definition)

AMD 规范的兴起主要是由于在 Web 应用中，它是另一种模块定义规范，与 CommonJS 有所不同。它专注于**浏览器端的异步加载**，旨在解决在浏览器环境中使用同步加载可能导致的性能问题。

在 AMD 规范中，模块的定义和加载是异步进行的，允许在页面加载过程中并行加载多个模块，而**不阻塞**页面的渲染。它使用 `define` 函数来定义模块，以及 `require` 函数来异步加载模块。**RequireJS** 是 AMD 规范的一个著名实现。

```typescript
// 定义模块
define(['dependency1', 'dependency2'], function(dep1, dep2) {
    // 模块的实现代码
    return {
        method1: function() {
            // ...
        },
        method2: function() {
            // ...
        }
    };
});

// 异步加载模块
require(['myModule'], function(myModule) {
    // 回调方式实现异步加载
    myModule.method1();
});
```

### CMD (Common Module Definition)

CMD 是另一种模块定义规范，与 CommonJS 和 AMD 有所不同。CMD 是由国内的开发者在 AMD 基础上提出的，**SeaJS** 是 CMD 规范的一个典型实现。CMD 主张一种更为**懒加载**和**就近依赖**的模块加载策略。

在 CMD 规范中，模块的加载是就近发生的，只有在需要使用某个模块时才会加载该模块，有助于减小页面的初始化加载时间。CMD 使用 `define` 函数定义模块，而模块内的依赖通过 `require` 来进行声明。

```typescript
// 定义模块
define(function(require, exports, module) {
    // 通过require声明依赖
    var dependency1 = require('dependency1');
    var dependency2 = require('dependency2');

    // 模块的实现代码
    exports.method1 = function() {
        // ...
    };

    exports.method2 = function() {
        // ...
    };
});

// 使用模块
define(function(require) {
    // 异步加载模块
    var myModule = require('myModule');
    myModule.method1();
});
```

### UMD (Universal Module Definition)

UMD 是一种**通用的模块化规范**，兼容 CommonJS、AMD、CMD 以及全局变量的使用，使模块可以在不同环境中运行。通常，UMD 模块会先检测当前运行环境支持哪种模块系统，然后根据检测结果采用相应的加载方式。这使得同一个模块既可以在浏览器端使用，也可以在服务器端使用，而无需修改代码。

```typescript
(function (root, factory) {
    if (typeof define === 'function' && define.cmd) {
        // CMD 环境
        define(function (require, exports, module) {
            var someDependency = require('someDependency');
            module.exports = factory(someDependency);
        });
    } else if (typeof define === 'function' && define.amd) {
        // AMD 环境
        define(['someDependency'], function (someDependency) {
            return factory(someDependency);
        });
    } else if (typeof exports === 'object') {
        // CommonJS 环境
        var someDependency = require('someDependency');
        module.exports = factory(someDependency);
    } else {
        // 全局变量
        root.YourModule = factory(root.SomeDependency);
    }
}(this, function (someDependency) {
    // 模块实现
    function yourModuleFunction() {
        // 使用 someDependency 进行一些操作
        console.log(someDependency);
    }

    // 暴露模块接口
    return {
        yourModuleFunction: yourModuleFunction
    };
}));
```

UMD 的出现是为了解决不同模块系统之间的兼容性问题，使得开发者能够编写一套代码，同时适用于多种加载环境。然而，随着 ES6 模块的普及，现代的开发中通常更倾向于使用原生的 ES6 模块语法，因为它在语法上更清晰，并且现代工具链对其提供了广泛的支持。

### ES Module

ES Module 是在 ES6 中引入的模块化规范，它在浏览器端和服务器端都能够被广泛支持，成为现代 JavaScript 开发的主流模块规范。

ES Module 使用 `export` 命令规定模块的对外接口，而模块内通过 `import` 命令用于输入其他模块提供的功能。大多数现代浏览器都支持 ES modules，可通过查看 [Can I use](https://caniuse.com/es6-module) 网站来查看不同浏览器对 ES modules 的支持情况。

相较于 CommonJS 等规范，它引入了一些优化和改进。

* **静态化设计**: ES Module 的设计思想是静态化，即**编译时加载**。在编译时就能确定模块的依赖关系、输入和输出的变量。这样的设计使得在运行时，引擎能够更快地处理模块加载，提高了执行效率。
*   **只读引用**: 相较于 CommonJS 的**值拷贝**，ES Module 采用的是**值引用**。当使用 import 命令引入模块时，得到的是一个只读引用，同时意味着模块内部的变化会反应在外部。

    ```typescript
    import {a} from './xxx.js'

    a = {}; // Syntax Error : 'a' is read-only;
    a.foo = 'hello'; // 合法操作
    ```
* **自动开启严格模式**：自动开启严格模式，提高了代码的安全性和可靠性。
*   **按需加载**: 当你使用 import 命令引入模块时，只有被引入的部分会被加载和执行，而不是整个模块。

    ```typescript
    import { stat, exists, readFile } from 'fs';
    // 只有 stat、readFile 和 exists 会被加载，其他部分不会被执行。
    ```
*   **动态加载模块**：使用 `import()` 函数可以在运行时动态加载模块，返回一个 Promise 对象。import() 类似于 Node.js 的 require() 方法，区别主要是前者是**异步加载**，后者是**同步加载**。

    ```typescript
    // 按需加载
    button.addEventListener('click', event => {
        import('./dialogBox.js')
            .then(dialogBox => {
                dialogBox.open();
            })
            .catch(error => {
                /* Error handling */
            });
    });

    // 条件加载
    if (condition) {
        import('moduleA').then(...);
    } else {
        import('moduleB').then(...);
    }

    // 动态模块路径
    import(f()).then(...);
    ```

### SystemJS

SystemJS 是一个模块加载器，它的设计目标是实现在**浏览器中动态加载模块**的能力。支持多种模块规范，包括 CommonJS、AMD、UMD、ES6 等。SystemJS 与工具如 **JSPM(JavaScript Package Manager)** 结合使用，实现在开发环境中的模块热更新。

```javascript
// 动态加载模块
System.import('./math.js').then(mathModule => {
    console.log(mathModule.add(2, 3)); // 输出: 5
});
```

尽管在现代 JavaScript 开发中，ES Module 已经成为主流规范，但 SystemJS 仍然在一些特殊场景或历史项目中发挥作用，为开发者提供了更大的灵活性。

***

JavaScript 的模块化发展经历了多个阶段，各有不同的模块化方案，适用于不同的场景。CommonJS 适用于服务器端，采用同步加载；AMD 适用于浏览器端，采用异步加载；CMD 强调懒加载和就近依赖；UMD 是通用规范，兼容多种加载环境；ES Module 是现代主流规范，支持静态化设计；SystemJS 提供浏览器端动态加载能力。
