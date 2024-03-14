# 最长连续序列

[**leetcode题目链接**](https://leetcode.cn/problems/longest-consecutive-sequence/description/)

给定一个未排序的整数数组 `nums` ，找出数字连续的最长序列（不要求序列元素在原数组中连续）的长度。

设计并实现时间复杂度为 `O(n)` 的算法解决此问题。

**示例 1：**

<pre><code><strong>输入：nums = [100,4,200,1,3,2]
</strong><strong>输出：4
</strong><strong>解释：最长数字连续序列是 [1, 2, 3, 4]。它的长度为 4。
</strong></code></pre>

**示例 2：**

<pre><code><strong>输入：nums = [0,3,7,2,5,8,4,6,0,1]
</strong><strong>输出：9
</strong></code></pre>

**提示：**

* `0 <= nums.length <= 105`
* `-109 <= nums[i] <= 109`

## 哈希表

**思路及算法**

将数组存入 Set 中，循环查找无左值的元素。再从当前元素开始循环找右值，直至找不到。

```typescript
function longestConsecutive(nums: number[]): number {
    if (nums.length === 0) {
        return 0;
    }

    const numSet = new Set(nums);
    let longestStreak = 0;

    for (const num of numSet) {
        // 如果当前数字是序列的起始数字（左边界）
        if (!numSet.has(num - 1)) {
            let currentNum = num;
            let currentStreak = 1;

            // 检查右边界
            // while 的循环次数与nums无关，所以 while 在这里的时间复杂度是 O(1)
            while (numSet.has(currentNum + 1)) {
                currentNum += 1;
                currentStreak += 1;
            }

            // 更新最长序列长度
            longestStreak = Math.max(longestStreak, currentStreak);
        }
    }

    return longestStreak;
}
```

**复杂度分析**

* 时间复杂度：O(n)
* 空间复杂度：O(n)
