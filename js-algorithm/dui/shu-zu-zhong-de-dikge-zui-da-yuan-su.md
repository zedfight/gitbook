# 数组中的第K个最大元素

[**leetcode 题目链接**](https://leetcode.cn/problems/kth-largest-element-in-an-array/description/)

给定整数数组 `nums` 和整数 `k`，请返回数组中第 **`k`** 个最大的元素。

请注意，你需要找的是数组排序后的第 `k` 个最大的元素，而不是第 `k` 个不同的元素。

你必须设计并实现时间复杂度为 `O(n)` 的算法解决此问题。

**示例 1:**

<pre><code><strong>输入: [3,2,1,5,6,4], k = 2
</strong><strong>输出: 5
</strong></code></pre>

**示例 2:**

<pre><code><strong>输入: [3,2,3,1,2,4,5,5,6], k = 4
</strong><strong>输出: 4
</strong></code></pre>

**提示：**

* `1 <= k <= nums.length <= 105`
* `-104 <= nums[i] <= 104`

## 快速排序

**思路及算法**

```typescript
function findKthLargest(nums: number[], k: number): number {
    
};
```

**复杂度分析**

* 时间复杂度：O(n)
* 空间复杂度：O(log⁡n)

