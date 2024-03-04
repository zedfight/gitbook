# 岛屿数量

[**leetcode 题目链接**](https://leetcode.cn/problems/number-of-islands/description/)

给你一个由 `'1'`（陆地）和 `'0'`（水）组成的的二维网格，请你计算网格中岛屿的数量。

岛屿总是被水包围，并且每座岛屿只能由水平方向和/或竖直方向上相邻的陆地连接形成。

此外，你可以假设该网格的四条边均被水包围。

**示例 1：**

<pre><code><strong>输入：grid = [
</strong>  ["1","1","1","1","0"],
  ["1","1","0","1","0"],
  ["1","1","0","0","0"],
  ["0","0","0","0","0"]
]
<strong>输出：1
</strong></code></pre>

**示例 2：**

<pre><code><strong>输入：grid = [
</strong>  ["1","1","0","0","0"],
  ["1","1","0","0","0"],
  ["0","0","1","0","0"],
  ["0","0","0","1","1"]
]
<strong>输出：3
</strong></code></pre>

**提示：**

* `m == grid.length`
* `n == grid[i].length`
* `1 <= m, n <= 300`
* `grid[i][j]` 的值为 `'0'` 或 `'1'`

## 深度优先搜索（DFS）

**思路及算法**

遍历二维数组，遇到`1`先统计入`islandCount`  ，然后递归其上下左右值是否为1，是的话置为0，继续递归其上下左右，若为0或不存在终止当前函数调用，直至递归完所有值。

```typescript
function numIslands(grid: string[][]): number {
    if (!grid || grid.length === 0) {
        return 0;
    }

    const rows = grid.length;
    const cols = grid[0].length;

    function dfs(row: number, col: number): void {
        if (row < 0 || row >= rows || col < 0 || col >= cols || grid[row][col] === '0') {
            return;
        }

        grid[row][col] = '0'; // Mark the current cell as visited

        // Explore adjacent cells
        dfs(row + 1, col);
        dfs(row - 1, col);
        dfs(row, col + 1);
        dfs(row, col - 1);
    }

    let islandCount = 0;

    for (let i = 0; i < rows; i++) {
        for (let j = 0; j < cols; j++) {
            if (grid[i][j] === '1') {
                islandCount++;
                dfs(i, j);
            }
        }
    }

    return islandCount;
}
```

**复杂度分析**

* 时间复杂度：$$O(MN)$$，其中 $$M$$ 和 $$N$$ 分别为行数和列数。
* 空间复杂度：$$O(MN)$$，主要是递归调用栈的消耗。

## **广度优先搜索（BFS）**

**思路及算法**

遍历二维数组，遇到`1`先统计入`islandCount`  ，然后在执行 bfs  函数，函数中定义一个数组，该数组存储当前`row`和`col`，判断当前位置是否为1，若是将其上下左右的位置继续存入数组，若不是直接进入下一次循环，重复遍历直至数组为空。

```typescript
function numIslands(grid: string[][]): number {
    if (!grid || grid.length === 0) {
        return 0;
    }

    const rows = grid.length;
    const cols = grid[0].length;

    function bfs(row: number, col: number): void {
        // 画图查看整条链路是斜着扫描。所以 queue 最长是min(M, N)
        const queue: number[][] = [];
        queue.push([row, col]);

        // while的循环次数与 row 和 col 无关，所以这里的时间复杂度是 O(1)
        while (queue.length > 0) {
            const current = queue.shift()!;
            const r = current[0];
            const c = current[1];

            if (r < 0 || r >= rows || c < 0 || c >= cols || grid[r][c] === '0') {
                continue;
            }

            grid[r][c] = '0'; // Mark the current cell as visited

            // Explore adjacent cells
            queue.push([r + 1, c]);
            queue.push([r - 1, c]);
            queue.push([r, c + 1]);
            queue.push([r, c - 1]);
        }
    }

    let islandCount = 0;

    for (let i = 0; i < rows; i++) {
        for (let j = 0; j < cols; j++) {
            if (grid[i][j] === '1') {
                islandCount++;
                bfs(i, j);
            }
        }
    }

    return islandCount;
}
```

**复杂度分析**

* 时间复杂度：$$O(MN)$$，其中 $$M$$ 和 $$N$$ 分别为行数和列数。
* 空间复杂度：$$O(min(M,N))$$，在最坏情况下，整个网格均为陆地，队列的大小可以达到 $$min(M,N)$$。

**深度优先搜索 (DFS)**

* **遍历顺序：** 从起始节点出发，尽可能深地访问每一个节点，直到达到最深的节点，然后再回溯到上一级节点，继续深入。
* **数据结构：** 通常使用递归调用或显式的栈来实现。
* **特点：** DFS更适用于深度遍历，适合解决路径类问题，例如找到两点之间的路径，或者寻找树的深度。

**广度优先搜索 (BFS)**

* **遍历顺序：** 从起始节点开始，逐层遍历，先访问当前层的所有节点，然后再访问下一层的节点。
* **数据结构：** 通常使用队列来实现。
* **特点：** BFS适用于层次遍历，例如寻找最短路径、连通性问题等
