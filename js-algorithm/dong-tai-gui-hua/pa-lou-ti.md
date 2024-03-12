# 爬楼梯

[**leetcode 题目链接**](https://leetcode.cn/problems/climbing-stairs/)

假设你正在爬楼梯。需要 `n` 阶你才能到达楼顶。

每次你可以爬 `1` 或 `2` 个台阶。你有多少种不同的方法可以爬到楼顶呢？

**示例 1：**

<pre><code><strong>输入：n = 2
</strong><strong>输出：2
</strong><strong>解释：有两种方法可以爬到楼顶。
</strong>1. 1 阶 + 1 阶
2. 2 阶
</code></pre>

**示例 2：**

<pre><code><strong>输入：n = 3
</strong><strong>输出：3
</strong><strong>解释：有三种方法可以爬到楼顶。
</strong>1. 1 阶 + 1 阶 + 1 阶
2. 1 阶 + 2 阶
3. 2 阶 + 1 阶
</code></pre>

**提示：**

* `1 <= n <= 45`

## 动态规划

**思路及算法**

动态规划的基本思路是将原问题分解成子问题，并保存子问题的解，以避免重复计算。本题可以理解为最后剩1阶和剩2阶到达楼顶的所有解是`n-1`和`n-2`的和。

```typescript
function climbStairs(n: number): number {
    if (n <= 0) {
        return 0;
    } else if (n === 1) {
        return 1;
    } else if (n === 2) {
        return 2;
    }

    const ways: number[] = [0, 1, 2];

    for (let i = 3; i <= n; i++) {
        ways[i] = ways[i - 1] + ways[i - 2];
    }

    return ways[n];
}
```

**复杂度分析**

* 时间复杂度：O(n)
* 空间复杂度：O(n)
