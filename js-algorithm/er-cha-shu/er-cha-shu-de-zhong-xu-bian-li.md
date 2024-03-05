# 二叉树的中序遍历

[**leetcode 题目链接**](https://leetcode.cn/problems/binary-tree-inorder-traversal/description/)

给定一个二叉树的根节点 `root` ，返回 _它的 **中序** 遍历_ 。

中序遍历（中根遍历）：先访问左子树，然后访问`根节点`， 最后访问右子树。

**示例 1：**

![](https://assets.leetcode.com/uploads/2020/09/15/inorder\_1.jpg)

<pre><code><strong>输入：root = [1,null,2,3]
</strong><strong>输出：[1,3,2]
</strong></code></pre>

**示例 2：**

<pre><code><strong>输入：root = []
</strong><strong>输出：[]
</strong></code></pre>

**示例 3：**

<pre><code><strong>输入：root = [1]
</strong><strong>输出：[1]
</strong></code></pre>

**提示：**

* 树中节点数目在范围 `[0, 100]` 内
* `-100 <= Node.val <= 100`

**进阶:** 递归算法很简单，你可以通过迭代算法完成吗？

## 递归算法

**思路及算法**

1. **初始化结果数组：** 声明一个数组 `result` 用于存储中序遍历的结果。
2. **递归中序遍历：** 定义内部函数 `traverse`，该函数接受一个节点作为参数，实现中序遍历的递归过程。具体步骤如下：
   * 若当前节点存在，则先递归遍历左子树（调用 `traverse(node.left)`）。
   * 将当前节点的值加入结果数组（`result.push(node.val)`）。
   * 再递归遍历右子树（调用 `traverse(node.right)`）。
3. **返回结果数组：** 最终返回存储中序遍历结果的数组 `result`。

```typescript
class TreeNode {
  val: number;
  left: TreeNode | null;
  right: TreeNode | null;

  constructor(val: number) {
    this.val = val;
    this.left = null;
    this.right = null;
  }
}

function inorderTraversal(root: TreeNode | null): number[] {
  const result: number[] = [];

  function traverse(node: TreeNode | null) {
    if (node) {
      traverse(node.left);
      result.push(node.val);
      traverse(node.right);
    }
  }

  traverse(root);

  return result;
}
```

**复杂度分析**

* 时间复杂度：$$O(N)$$，其中 $$N$$ 为二叉树节点的个数。二叉树的遍历中每个节点会被访问一次且只会被访问一次。
* 空间复杂度：$$O(N)$$，空间复杂度取决于递归的栈深度，而栈深度在二叉树为一条链的情况下会达到 $$O(n)$$ 的级别。

