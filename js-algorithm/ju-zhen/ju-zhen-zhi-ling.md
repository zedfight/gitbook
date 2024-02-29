# 矩阵置零

[**leetcode 题目链接**](https://leetcode.cn/problems/set-matrix-zeroes/description/)

给定一个 _`m`_` ``x`` `_`n`_ 的矩阵，如果一个元素为 **0** ，则将其所在行和列的所有元素都设为 **0** 。请使用 [**原地**](http://baike.baidu.com/item/%E5%8E%9F%E5%9C%B0%E7%AE%97%E6%B3%95) 算法**。**

原地算法（In-place algorithm）是一种在执行过程中使用固定的、有限的额外空间的算法。这意味着算法的运行不需要额外的内存空间。相反，它会在原始输入数据的内存空间上进行操作，而不是创建新的数据结构。

**示例 1：**

![](https://assets.leetcode.com/uploads/2020/08/17/mat1.jpg)

<pre><code><strong>输入：matrix = [[1,1,1],[1,0,1],[1,1,1]]
</strong><strong>输出：[[1,0,1],[0,0,0],[1,0,1]]
</strong></code></pre>

**示例 2：**

![](https://assets.leetcode.com/uploads/2020/08/17/mat2.jpg)

<pre><code><strong>输入：matrix = [[0,1,2,0],[3,4,5,2],[1,3,1,5]]
</strong><strong>输出：[[0,0,0,0],[0,4,5,0],[0,3,1,0]]
</strong></code></pre>

**提示：**

* `m == matrix.length`
* `n == matrix[0].length`
* `1 <= m, n <= 200`
* `-231 <= matrix[i][j] <= 231 - 1`

## 原地算法

**思路及算法**

1. **初始化：** 获取矩阵的行数 `m` 和列数 `n`，以及两个标志变量 `firstRowHasZero` 和 `firstColHasZero`，用于记录第一行和第一列是否有零。
2. **检查第一行和第一列：** 通过遍历第一行和第一列，检查是否存在零元素，如果存在则相应地更新标志变量 `firstRowHasZero` 和 `firstColHasZero`。
3. **标记零元素位置：** 从矩阵的第二行和第二列开始遍历，如果某个元素为零，则将对应的第一行和第一列的元素置零，用它们来记录该行和该列是否需要被置零。
4. **置零：** 根据第一行和第一列的标志，遍历矩阵的除第一行和第一列之外的部分，将对应的行和列置零。
5. **处理第一行和第一列：** 根据前面的标志变量 `firstRowHasZero` 和 `firstColHasZero`，如果需要将第一行或第一列置零，则分别遍历并更新它们的元素。

```typescript
function setZeroes(matrix: number[][]): void {
    const m = matrix.length;
    const n = matrix[0].length;

    let firstRowHasZero = false;
    let firstColHasZero = false;

    // 检查第一行是否有零
    for (let j = 0; j < n; j++) {
        if (matrix[0][j] === 0) {
            firstRowHasZero = true;
            break;
        }
    }

    // 检查第一列是否有零
    for (let i = 0; i < m; i++) {
        if (matrix[i][0] === 0) {
            firstColHasZero = true;
            break;
        }
    }
    
    // firstRowHasZero 和 firstRowHasZero 优先标记是为了记录首行和首列是否全部置为0。如果不先标记后续的操作会影响首行首列是否需要置为0

    // 一行一行的遍历，若当前元素为0，则该元素的最左侧和最上侧置为0
    // 相当于标记所在行和所在列是否需要置为0
    for (let i = 1; i < m; i++) {
        for (let j = 1; j < n; j++) {
            if (matrix[i][j] === 0) {
                matrix[i][0] = 0;
                matrix[0][j] = 0;
            }
        }
    }

    // 一行一行的遍历，如果当前位置的最左侧或者最上侧为0，则当前位置置为0
    // 相当于执行所在行和所在列全部置为0
    for (let i = 1; i < m; i++) {
        for (let j = 1; j < n; j++) {
            if (matrix[i][0] === 0 || matrix[0][j] === 0) {
                matrix[i][j] = 0;
            }
        }
    }

    // 除首行首列之外所有元素置0后再开始首行首列置0
    // 如果第一行需要置零，将第一行置零
    if (firstRowHasZero) {
        for (let j = 0; j < n; j++) {
            matrix[0][j] = 0;
        }
    }

    // 如果第一列需要置零，将第一列置零
    if (firstColHasZero) {
        for (let i = 0; i < m; i++) {
            matrix[i][0] = 0;
        }
    }
}
```

**复杂度分析**

* 时间复杂度：$$O(M*N)$$
* 空间复杂度：$$O(1)$$
