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

## 快速选择算法

**思路及算法**

快速选择是一种基于快速排序思想的算法，该算法通过每次选择一个枢纽元素（pivot）并对数组进行分区，将比枢纽元素大的元素移动到它的左侧，比它小的元素移动到右侧。根据分区的结果，可以确定枢纽元素的最终位置，即它是数组中的第几大元素。

```typescript
function findKthLargest(nums: number[], k: number): number {
    // 辅助函数：交换数组中两个元素的位置
    function swap(arr: number[], i: number, j: number): void {
        const temp = arr[i];
        arr[i] = arr[j];
        arr[j] = temp;
    }

    // 辅助函数：进行一次快速选择的分区操作
    // 将arr[hight] 作为枢纽值，大于该值的都在其左侧，小于该值的都在其右侧
    function partition(arr: number[], low: number, high: number): number {
        const pivot = arr[high]; // 选择最后一个元素作为枢纽元素
        let i = low;

        for (let j = low; j < high; j++) {
            if (arr[j] >= pivot) {
                swap(arr, i, j);
                i++;
            }
        }

        swap(arr, i, high); // 将枢纽元素放到正确的位置
        return i;
    }

    // 快速选择算法
    function quickSelect(arr: number[], low: number, high: number, k: number): number {
        const partitionIndex = partition(arr, low, high);

        if (partitionIndex === k - 1) {
            return arr[partitionIndex];
        } else if (partitionIndex < k - 1) {
            return quickSelect(arr, partitionIndex + 1, high, k);
        } else {
            return quickSelect(arr, low, partitionIndex - 1, k);
        }
    }

    // 调用快速选择算法找到第 k 个最大元素
    return quickSelect(nums, 0, nums.length - 1, k);
}
```

**复杂度分析**

* 时间复杂度：O(n)，虽然递归使用的for循环，但是在循环后才开始递归，所以复杂度不累加
* 空间复杂度：O(log⁡n)，递归使用调用栈的空间复杂度为 $$O(log⁡n)$$

## **快速排序**

```typescript
var findKthLargest = function(nums, k) {
    return nums.sort((a,b) => b-a)[k - 1]
};
```

**复杂度分析**

* 时间复杂度：O(n\*log n)
* 空间复杂度：O(log⁡n)
