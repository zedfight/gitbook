# 买卖股票的最佳时机

[**leetcode 题目链接**](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock/description/)

给定一个数组 `prices` ，它的第 `i` 个元素 `prices[i]` 表示一支给定股票第 `i` 天的价格。

你只能选择 **某一天** 买入这只股票，并选择在 **未来的某一个不同的日子** 卖出该股票。设计一个算法来计算你所能获取的最大利润。

返回你可以从这笔交易中获取的最大利润。如果你不能获取任何利润，返回 `0` 。

**示例 1：**

<pre><code><strong>输入：[7,1,5,3,6,4]
</strong><strong>输出：5
</strong><strong>解释：在第 2 天（股票价格 = 1）的时候买入，在第 5 天（股票价格 = 6）的时候卖出，最大利润 = 6-1 = 5 。
</strong>     注意利润不能是 7-1 = 6, 因为卖出价格需要大于买入价格；同时，你不能在买入前卖出股票。
</code></pre>

**示例 2：**

<pre><code><strong>输入：prices = [7,6,4,3,1]
</strong><strong>输出：0
</strong><strong>解释：在这种情况下, 没有交易完成, 所以最大利润为 0。
</strong></code></pre>

**提示：**

* `1 <= prices.length <= 105`
* `0 <= prices[i] <= 104`

## 贪心算法

**思路及算法**

贪心算法的核心思想是每一步都选择当前状态下最优的解，而不考虑全局的最优解（类似动态规划的思想）。最大利润的差值一定是在`[0, i-1]` 之间选最低点买入；所以遍历数组，依次求每个卖出时机的的最大差值，再从中取最大值。（画出折线图更清晰）。

按顺序寻找最小值存储为`minPrice`，寻找更大值`当前值 - minPrice`存为`maxProfit`。

```typescript
function maxProfit(prices: number[]): number {
    if (prices.length <= 1) {
        return 0; // 无法进行交易，返回0
    }

    let minPrice = prices[0]; // 记录最低价格
    let maxProfit = 0; // 记录最大利润

    for (let i = 1; i < prices.length; i++) {
        // 更新最低价格
        minPrice = Math.min(minPrice, prices[i]);

        // 计算当前价格卖出时的利润，并更新最大利润
        maxProfit = Math.max(maxProfit, prices[i] - minPrice);
    }

    return maxProfit;
}
```

**复杂度分析**

* 时间复杂度：O(n)
* 空间复杂度：O(1)
