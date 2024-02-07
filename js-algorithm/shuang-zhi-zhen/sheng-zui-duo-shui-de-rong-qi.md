# 盛最多水的容器

[**leetcode题目链接**](https://leetcode.cn/problems/container-with-most-water/description/)

给定一个长度为 `n` 的整数数组 `height` 。有 `n` 条垂线，第 `i` 条线的两个端点是 `(i, 0)` 和 `(i, height[i])` 。

找出其中的两条线，使得它们与 `x` 轴共同构成的容器可以容纳最多的水。

返回容器可以储存的最大水量。

**说明：**不能倾斜容器。

**示例 1：**

![](https://aliyun-lc-upload.oss-cn-hangzhou.aliyuncs.com/aliyun-lc-upload/uploads/2018/07/25/question\_11.jpg)

<pre><code><strong>输入：[1,8,6,2,5,4,8,3,7]
</strong><strong>输出：49
</strong><strong>解释：图中垂直线代表输入数组 [1,8,6,2,5,4,8,3,7]。在此情况下，容器能够容纳水（表示为蓝色部分）的最大值为 49。
</strong></code></pre>

**示例 2：**

<pre><code><strong>输入：height = [1,1]
</strong><strong>输出：1
</strong></code></pre>

**提示：**

* `n == height.length`
* `2 <= n <= 105`
* `0 <= height[i] <= 104`

## **双指针**

**思路及算法**

1. 初始化变量`ans`（用于存储最大面积）、`left`（表示最左边的索引）和`right`（表示最右边的索引）。
2. 启动一个while循环，条件为`left < right`，表示搜索最大面积的过程一直持续，直到左右索引相遇或交叉。
3. 使用公式计算当前容器形成的面积：`area = (right - left) * Math.min(height[left], height[right])`。这个公式根据宽度（右索引和左索引的差异）和两个垂直线之间的最小高度来计算面积。
4. 通过取当前的`ans`和计算得到的`area`的最大值来更新最大面积（`ans`）。
5. 将具有较小高度的索引朝向数组的中心移动。如果`height[left]`小于`height[right]`，则增加`left`；否则，减小`right`。这一步对于确保我们探索不同的容器可能性并朝着可能更大的高度前进是至关重要的。
6. 重复步骤3-5，直到`left`不再小于`right`。

```typescript
var maxArea = function(height) {
    let ans = 0, left = 0, right = height.length - 1;
    while (left < right) {
        const area = (right - left) * Math.min(height[left], height[right]);
        ans = Math.max(ans, area);
        if (height[left] < height[right]) {
            left++;
        } else {
            right--;
        }
    }
    return ans;
};
```

**复杂度分析**&#x20;

* 时间复杂度：O(n)
* 空间复杂度：O(1)
