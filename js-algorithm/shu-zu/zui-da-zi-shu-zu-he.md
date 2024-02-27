# 最大子数组和

[**leetcode 题目链接**](https://leetcode.cn/problems/maximum-subarray/)

给你一个整数数组 `nums` ，请你找出一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。

**子数组** 是数组中的一个连续部分。

**示例 1：**

<pre><code><strong>输入：nums = [-2,1,-3,4,-1,2,1,-5,4]
</strong><strong>输出：6
</strong><strong>解释：连续子数组 [4,-1,2,1] 的和最大，为 6 。
</strong></code></pre>

**示例 2：**

<pre><code><strong>输入：nums = [1]
</strong><strong>输出：1
</strong></code></pre>

**示例 3：**

<pre><code><strong>输入：nums = [5,4,-1,7,8]
</strong><strong>输出：23
</strong></code></pre>

**提示：**

* `1 <= nums.length <= 105`
* `-104 <= nums[i] <= 104`

**进阶：**如果你已经实现复杂度为 `O(n)` 的解法，尝试使用更为精妙的**分治法**求解。

## 动态规划

**思路及算法**

循环遍历数组，并维护两个变量 `currentSum` 和 `maxSum`，分别表示当前子数组的和和最大子数组的和。每次遍历中的当前子数组和与当前下标数组作比较。如果数组和大于下标值说明当前下标是正作用继续累加，如果小于当前下标值说明是副作用，则抛弃以往从当前位置往后重新计算子数组和。

1. 初始化两个变量：`maxSum` 用于存储最大子数组和，`currentSum` 用于存储当前子数组的和，初始值都设为数组的第一个元素 `nums[0]`。
2. 从数组的第二个元素开始遍历，对于每个元素，更新 `currentSum` 的值，使其表示包含当前元素的最大子数组和。如果 `currentSum` 变为负数，说明当前子数组的和对于后续元素没有增益，应该舍弃，重新从当前元素开始计算子数组和。
3. 在每一步迭代中，比较 `currentSum` 和 `maxSum`，将 `maxSum` 更新为两者中的较大值，以保持记录最大的子数组和。
4. 最终返回 `maxSum`，即为数组的最大连续子数组和。

```typescript
function maxSubArray(nums: number[]): number {
    if (nums.length === 0) {
        return 0;
    }

    let maxSum = nums[0];
    let currentSum = nums[0];

    for (let i = 1; i < nums.length; i++) {
        // 如果当前子数组和为负数，就抛弃之前的子数组，从当前元素重新开始
        currentSum = Math.max(nums[i], currentSum + nums[i]);

        // 记录最大子数组和
        maxSum = Math.max(maxSum, currentSum);
    }

    return maxSum;
}
```

**复杂度分析**

* 时间复杂度：$$O(N)$$
* 空间复杂度：$$O(1)$$

## **分治法**

**思路及算法**

**复杂度分析**
