# 搜索插入位置

[**leetcode 题目链接**](https://leetcode.cn/problems/search-insert-position/description/)

给定一个排序数组和一个目标值，在数组中找到目标值，并返回其索引。如果目标值不存在于数组中，返回它将会被按顺序插入的位置。

请必须使用时间复杂度为 `O(log n)` 的算法。

**示例 1:**

<pre><code><strong>输入: nums = [1,3,5,6], target = 5
</strong><strong>输出: 2
</strong></code></pre>

**示例 2:**

<pre><code><strong>输入: nums = [1,3,5,6], target = 2
</strong><strong>输出: 1
</strong></code></pre>

**示例 3:**

<pre><code><strong>输入: nums = [1,3,5,6], target = 7
</strong><strong>输出: 4
</strong></code></pre>

**提示:**

* `1 <= nums.length <= 104`
* `-104 <= nums[i] <= 104`
* `nums` 为 **无重复元素** 的 **升序** 排列数组
* `-104 <= target <= 104`

## 二分法

**思路及算法**

不断将搜索范围缩小一半来快速定位目标值。

考虑两个极端情况：

1. target 在 left 和 right 的右侧，所以需要 while (left <= right) 而不是 while(left\<right);
2. target 在left 和 right 的左侧，所以只能 return left 而不是 return right。

```typescript
function searchInsert(nums: number[], target: number): number {
    let left = 0;
    let right = nums.length - 1;

    while (left <= right) {
        const mid = Math.floor((left + right) / 2);

        if (nums[mid] === target) {
            return mid;
        } else if (nums[mid] < target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    
    // 如果目标值不存在，返回应插入的位置
    return left;
}
```

**复杂度分析**

* 时间复杂度：O(log n)，使用了二分查找算法，每一步都将搜索范围缩小一半，因此时间复杂度是对数级别的。
* 空间复杂度：O(1)

