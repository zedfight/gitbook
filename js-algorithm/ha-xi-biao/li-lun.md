# 理论

在 JavaScript 中对象是一种数据结构，用于实现键-值对的存储和检索。这种数据结构也被称作映射、字典和哈希表。ES6 中的 Map 和 Set 也是基于哈希表实现的数据结构，提供了更高级的键-值对管理功能。

一个典型的 JavaScript 对象如下所示：

```js
const obj = {
    prop1: "I'm",
    prop2: "an",
    prop3: "object",
    prop4: function() {
        console.log("I'm a property!");
    }
};
```

JavaScript 对象也有内置的方法供我们进行不同的操作，或者获取特定对象的信息，完整内容可以查看[这里](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global\_Objects/Object)。

在大 O 表示法中，对象操作的**时间复杂度**如下：

* 访问对象属性：O(1)
* 添加对象属性：O(1)
* 删除对象属性：O(1)
* 修改对象属性：O(1)
* 遍历对象属性：O(n)
