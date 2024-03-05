# 全排列

[**leetcode 题目链接**](https://leetcode.cn/problems/permutations/description/)

给定一个不含重复数字的数组 `nums` ，返回其 _所有可能的全排列_ 。你可以 **按任意顺序** 返回答案。

全排列数组数等于`nums.lenght!`

**示例 1：**

<pre><code><strong>输入：nums = [1,2,3]
</strong><strong>输出：[[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
</strong></code></pre>

**示例 2：**

<pre><code><strong>输入：nums = [0,1]
</strong><strong>输出：[[0,1],[1,0]]
</strong></code></pre>

**示例 3：**

<pre><code><strong>输入：nums = [1]
</strong><strong>输出：[[1]]
</strong></code></pre>

**提示：**

* `1 <= nums.length <= 6`
* `-10 <= nums[i] <= 10`
* `nums` 中的所有整数 **互不相同**

## 回溯算法

**思路及算法**

回溯算法的核心是在 for 循环中递归，每次递归都在做选择、递归、撤销选择这三个操作。

```typescript
function permute(nums: number[]): number[][] {
    const result: number[][] = [];

    function backtrack(current: number[]): void {
        if (current.length === nums.length) {
            // 当当前排列长度等于数组长度时，将其加入结果集
            result.push([...current]);
            return;
        }

        for (const num of nums) {
            // 如果当前数字已经在当前排列中，则跳过
            if (current.includes(num)) {
                continue;
            }

            // 将当前数字加入排列中
            current.push(num);

            // 递归调用，生成下一个数字的排列
            backtrack(current);

            // 回溯，撤销当前数字的选择，以便尝试其他可能性
            current.pop();
        }
    }

    // 从空排列开始生成全排列
    backtrack([]);

    return result;
}
```

**复杂度分析**

* 时间复杂度：$$O(n!)$$，其中 $$n$$ 为序列的长度，每递归一层都会相应减少一次循环
* 空间复杂度：O(n\*n!)，由于 result 是二维数组，大小等于`nums.lenght * 排列格式`

