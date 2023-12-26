# 探索 JavaScript AST

<figure><img src="../.gitbook/assets/javascript-ast.jpeg" alt=""><figcaption></figcaption></figure>

抽象语法树（Abstract Syntax Tree ，AST）是代码的抽象表示形式，以树状结构呈现代码的语法结构。在前端领域，AST有着重要的作用，它为前端开发者提供了强大的工具来提高代码质量、性能和开发效率，确保代码的可维护性和跨浏览器兼容性。

### AST 的结构

简单回顾一下程序语言的基础知识，JavaScript 作为一个解释型语言。解释器把高级语言翻译成目标程序，通常分为两个部分：

```
分析阶段：此阶段进行词法、语法和语义分析

解释执行阶段：直接解释执行源代码
```

在JavaScript解释器分析代码时，词法分析阶段将源代码拆分成标记，语法分析阶段将标记组织成树状结构，这就是AST。

AST 对象中包含了哪些信息呢？我们可以通过[AST Explorer工具查看](https://astexplorer.net/)，作为开发者理解和掌握 AST 节点是核心。AST 中有多种不同类型的节点，每种节点代表了代码中的不同语法或语义结构。以下是一些常见的节点类型：

```
标识符节点（Identifier）：用于表示变量名、函数名等标识符。这些节点通常包含标识符的名称。e.g: x、myFunction

文字节点（Literal）：用于表示常量或字面值，如数字、字符串、布尔值等。这些节点通常包含字面值的值。e.g: "Hello World"、true

表达式节点（Expression）：用于表示各种表达式，例如算术表达式、逻辑表达式、函数调用等。这些节点通常包含操作符和操作数。e.g: x + 1、myFunction()

语句节点（Statement）：用于表示语句，例如赋值语句、条件语句、循环语句等。这些节点通常包含关键字和相关的表达式或块。e.g: if (condition) { /* code */ }、for (let i = 0; i < 10; i++) { /* code */ }

函数节点（Function）：用于表示函数定义，包括函数名、参数列表和函数体。这些节点通常包含函数的相关信息。e.g: function myFunction(x, y) { /* code */ }

对象节点（Object）：用于表示对象字面量，包括对象的属性和属性值。这些节点通常以键值对的形式组织。e.g: { x: 1, y: 2 }

数组节点（Array）：用于表示数组字面量，包括数组的元素。这些节点通常包含一个元素列表。e.g: [1, 2, 3]
```

### 构建和遍历 AST

JavaScipt 解释器是开发者无法直接操作的，但我们可以使用相应的工具来构建和遍历 AST，以下以 babel 为例，演示如何将加法函数转换为乘法函数的过程：

1. 初始化项目并安装依赖：

```bash
 mkdir babel-ast-demo && cd babel-ast-demo && yarn init -y && yarn add @babel/core @babel/preset-env --dev
```

2.  构建 JavaScript AST

    ```js
    // index.js
    const babel = require('@babel/core');

    const code = `
    function add(a, b) {
        return a + b;
    }
    `;

    // 解析代码并生成 AST
    const ast = babel.parse(code, {
    sourceType: 'module',
    });

    console.log(ast);
    ```
3.  遍历、访问和修改节点

    ```js
    // 遍历 AST
    babel.traverse(ast, {
        //  访问入参节点
        Identifier(path) {
            const node = path.node;
            // 修改入参名称a
            if (node.name === 'a') {
                node.name = 'x';
            }
            // 修改入参名称b
            if (node.name === 'b') {
                node.name = 'y';
            }
        },

        // 访问函数声明节点
        FunctionDeclaration(path) {
            const node = path.node;   
            // 修改函数名称  
            node.id.name = 'multiply';   
            // 修改函数体
            node.body.body = [
                babel.template.statement.ast('x*y'),
            ];
        },
    });

    ```
4. 生成源码

```js
    // 生成修改后的代码
    const { code: modifiedCode } = babel.transformFromAst(ast, null, {
        presets: ['@babel/preset-env'], 
    });

    console.log(modifiedCode); // function multiply(x, y) { return x * y; }
```

完整代码请查看 [babel-ast-demo](https://github.com/vocoWone/babel-ast-demo)

### AST 的价值

```
JavaScript AST 在前端开发中具有多种重要应用：

代码的转换： AST 可以用于代码转换，将新的 ECMAScript 特性转换为向后兼容的代码，确保代码在旧版本的 JavaScript 引擎上运行。例如，Babel 就是一个广泛使用 AST 进行代码转换的工具，它可以将新版 JavaScript 转换为向后兼容的代码，以确保在不同环境中运行。

性能优化： AST 可用于分析代码的结构并进行优化，去除不必要的代码或者提取重复的代码以减小文件大小，以提高应用程序的性能。Webpack 利用 AST 实现了 Tree Shaking，用于去除未使用的代码，优化打包后的文件大小。

代码检查： AST 允许工具进行静态代码分析，进行代码质量检查，发现潜在的错误、漏洞和不规范的代码。eslint/stylelint 是常用的工具，它通过 AST 进行代码质量检查，可定制的规则集能够适应不同的项目需求。

框架的实现： 许多前端框架和库都依赖于 AST 来实现组件化、模板解析和虚拟 DOM。React 就是利用 AST 分析 JSX 语法并将其转换为 JavaScript 函数调用。

编辑器增强：编辑器利用 AST 实现智能提示、自动补全、代码高亮、重构功能等，提高开发效率。例如，VS Code 中的 "Rename Symbol" 功能使用了抽象语法树（AST）来实现。
```

### JavaScript AST 的崭新应用

除了上述提到的应用，JavaScript AST还有一个令人兴奋的新应用 —— [dot-i18n](https://github.com/FE-Combo/dot-i18n)。它利用 AST 实现多语言的无痕配置极大提升了开发者的幸福感。以下将 dot-i18n 与 create-react-app 模板结合使用的简要步骤：

1.  初始化项目

    ```bash
    npx create-react-app dot-i18n-demo --template typescript
    ```
2.  安装所需依赖

    ```bash
    cd dot-i18n-demo
    yarn eject
    yarn add dot-i18n --save 
    yarn add dot-i18n-loader --dev
    ```
3. 根目录创建i18n.config.json

```json
{
    "baseUrl": "/src/", // 项目根目录
    "outDir": "/src/locales", // 词条输出目录
    "filename": "index", // 词条文件名
    "exportExcelPath": "/.i18n/result.xlsx", // 词条导出excel路径
    "importExcelPath": "/.i18n/result.xlsx", // 词条导入excel路径
    "languages": ["zh", "en"] // 词条语种，数组[0]代表第一语种
}
```

4. package.json 中添加相应的脚本

```json
"scripts": {
  "ts2excel": "node ./node_modules/dot-i18n-loader/node/ts2excel", // 初始化 locales 文件或扫描词条
  "excel2ts": "node ./node_modules/dot-i18n-loader/node/excel2ts", // 将 locales 中的词条导出到 excel 中
  "scanning": "node ./node_modules/dot-i18n-loader/node/scanning" // 将 excel 中的词条导入到 locales 中
}
```

5. 配置Webpack Loader

```js
[
    ...loaders,
    {
    test: /\.(tsx|ts)$/,
    exclude: /node_modules/,
    use: { loader: 'dot-i18n-loader' }
    }
]
```

如果使用 Webpack 5+，还需配置额外的插件

```js
[
    ...plugins,
    new webpack.ProvidePlugin({
        Buffer: ['buffer', 'Buffer']
    }),
]
```

6. 初始化词条，执行 `yarn scanning` 初始化 locales 文件
7. 在根组件中使用 DotI18N.Provider 包裹您的应用

```typescript
// index.tsx
import DotI18N from "dot-i18n";
import locales from "../locales";

<DotI18N.Provider locales={locales.zh}>
    <App />
</DotI18N.Provider>
```

8. 在 tsconfig.json 中导入相关类型

```js
// global.d.ts
import("dot-i18n/global")

// tsconfig.js
{
"compilerOptions": {
    "typeRoots": ["./node_modules/@types/", "./global.d.ts"],
},
"exclude": ["node_modules"]
}
```

9. 在项目中编写多语言文本，更多框架特性请[查阅](https://github.com/FE-Combo/dot-i18n)

```typescript
// App.tsx
import logo from './logo.svg';
import './App.css';

function App() {
return (
    <div className="App">
    <header className="App-header">
        <img src={logo} className="App-logo" alt="logo" />
        <div>
            <i18n>我是xml形式多语言词条，性能更优</i18n>
        </div>
        <div>
            {i18n("我是函数式多语言词条，使用更灵活")}
        </div>
        -------------------------------------------
        <div>
            <i18n namespace="my">我有自己的命名空间</i18n>
        </div>
        <div>
            {i18n("我也有自己的命名空间", {namespace: "ym"})}
        </div>
        -------------------------------------------
        <div>
            <i18n>我不支持变量</i18n>
        </div>
        <div>
            {i18n("但是我支持变量{value}", {replace: {"{value}": "--新值--"}})}
        </div>
    </header>
    </div>
);
}

export default App;
```

10. 翻译流程如下： 执行 yarn scanning（词条扫描）：项目中的词条收集到locales中 执行 yarn ts2excel（词条导出excel）：将locales中的词条导出到excel中 执行yarn excel2ts（词条导入）：将excel中的词条导入到locales中

通过以上配置和步骤，您可以在项目中轻松使用多语言功能。如果想查看完整的示例代码，请访问 [dot-i18n-demo](https://github.com/vocoWone/dot-i18n-demo)

如果你对这个工具感兴趣，欢迎点击 Star 和提出 PR，共同改进和推进它的发展，从而提升我们的开发体验。

### 结语

JavaScript AST 是前端开发中不可或缺的工具，它有助于编写高质量、高性能、易维护的代码。深入了解和利用 AST 将使前端开发更具有创造性和生产力。
