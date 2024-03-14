# 只出现一次数字

[**leetcode题目链接**](https://leetcode.cn/problems/single-number/description/)

给你一个 **非空** 整数数组 `nums` ，除了某个元素只出现一次以外，其余每个元素均出现两次。找出那个只出现了一次的元素。

你必须设计并实现线性时间复杂度的算法来解决此问题，且该算法只使用常量额外空间。&#x20;

**示例 1 ：**

<pre><code><strong>输入：nums = [2,2,1]
</strong><strong>输出：1
</strong></code></pre>

**示例 2 ：**

<pre><code><strong>输入：nums = [4,1,2,1,2]
</strong><strong>输出：4
</strong></code></pre>

**示例 3 ：**

<pre><code><strong>输入：nums = [1]
</strong><strong>输出：1
</strong></code></pre>

**提示：**

* `1 <= nums.length <= 3 * 104`
* `-3 * 104 <= nums[i] <= 3 * 104`
* 除了某个元素只出现一次以外，其余每个元素均出现两次。

## 异或

**思路及算法**

相同的元素异或得0，而任何数与0异或仍得其本身。转换成二进制进行计算。

```typescript
function singleNumber(nums: number[]): number {
    let result = 0;
    for (let num of nums) {
        result ^= num;
    }
    return result;
}
```

**复杂度分析**

* 时间复杂度：O(n)
* 空间复杂度：O(1)
