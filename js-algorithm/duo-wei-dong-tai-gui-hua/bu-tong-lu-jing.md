# 不同路径

[**leetcode题目链接**](https://leetcode.cn/problems/unique-paths/description/)

一个机器人位于一个 `m x n` 网格的左上角 （起始点在下图中标记为 “Start” ）。

机器人每次只能向下或者向右移动一步。机器人试图达到网格的右下角（在下图中标记为 “Finish” ）。

问总共有多少条不同的路径？&#x20;

**示例 1：**

![](https://pic.leetcode.cn/1697422740-adxmsI-image.png)

<pre><code><strong>输入：m = 3, n = 7
</strong><strong>输出：28
</strong></code></pre>

**示例 2：**

<pre><code><strong>输入：m = 3, n = 2
</strong><strong>输出：3
</strong><strong>解释：
</strong>从左上角开始，总共有 3 条路径可以到达右下角。
1. 向右 -> 向下 -> 向下
2. 向下 -> 向下 -> 向右
3. 向下 -> 向右 -> 向下
</code></pre>

**示例 3：**

<pre><code><strong>输入：m = 7, n = 3
</strong><strong>输出：28
</strong></code></pre>

**示例 4：**

<pre><code><strong>输入：m = 3, n = 3
</strong><strong>输出：6
</strong></code></pre>

**提示：**

* `1 <= m, n <= 100`
* 题目数据保证答案小于等于 `2 * 109`

## 动态规划

**思路及算法**

动态方程：`dp[i][j] = dp[i-1][j] + dp[i][j-1]`。`dp[i][j]`只能由其上侧向下移动一位和其左侧向右移动一位所得。所以`dp[i][j]`的路径是这两个方案路径的和。

对于第一行 `dp[0][j]`和第一列 `dp[i][0]`，由于都是在边界，所以只能为 `1`。

```typescript
function uniquePaths(m: number, n: number): number {
    // 创建一个二维数组来存储路径数量
    const dp: number[][] = new Array(m).fill(0).map(() => new Array(n).fill(0));

    // 初始化边界条件
    for (let i = 0; i < m; i++) {
        dp[i][0] = 1; // 第一列只能向下移动，所以只有一种路径
    }

    for (let j = 0; j < n; j++) {
        dp[0][j] = 1; // 第一行只能向右移动，所以只有一种路径
    }

    // 填充其余的格子，每个格子的路径数量等于其左侧格子和上方格子的路径数量之和
    for (let i = 1; i < m; i++) {
        for (let j = 1; j < n; j++) {
            dp[i][j] = dp[i - 1][j] + dp[i][j - 1];
        }
    }

    // 返回右下角格子的路径数量
    return dp[m - 1][n - 1];
}
```

**复杂度分析**

* 时间复杂度：O(mn)
* 空间复杂度：O(mn)
