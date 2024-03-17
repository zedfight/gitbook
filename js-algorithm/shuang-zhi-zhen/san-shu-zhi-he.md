# 三数之和

[**leetcode题目链接**](https://leetcode.cn/problems/3sum/description/)

给你一个整数数组 `nums` ，判断是否存在三元组 `[nums[i], nums[j], nums[k]]` 满足 `i != j`、`i != k` 且 `j != k` ，同时还满足 `nums[i] + nums[j] + nums[k] == 0` 。请

你返回所有和为 `0` 且不重复的三元组。

**注意：**答案中不可以包含重复的三元组。

**示例 1：**

<pre><code><strong>输入：nums = [-1,0,1,2,-1,-4]
</strong><strong>输出：[[-1,-1,2],[-1,0,1]]
</strong><strong>解释：
</strong>nums[0] + nums[1] + nums[2] = (-1) + 0 + 1 = 0 。
nums[1] + nums[2] + nums[4] = 0 + 1 + (-1) = 0 。
nums[0] + nums[3] + nums[4] = (-1) + 2 + (-1) = 0 。
不同的三元组是 [-1,0,1] 和 [-1,-1,2] 。
注意，输出的顺序和三元组的顺序并不重要。
</code></pre>

**示例 2：**

<pre><code><strong>输入：nums = [0,1,1]
</strong><strong>输出：[]
</strong><strong>解释：唯一可能的三元组和不为 0 。
</strong></code></pre>

**示例 3：**

<pre><code><strong>输入：nums = [0,0,0]
</strong><strong>输出：[[0,0,0]]
</strong><strong>解释：唯一可能的三元组和为 0 。
</strong></code></pre>

**提示：**

* `3 <= nums.length <= 3000`
* `-105 <= nums[i] <= 105`

## 排序+双指针

**思路及算法**

暴力法搜索为 $$O(N^3)$$ 时间复杂度，可通过双指针动态消去无效解来优化效率。

先对数组进行排序，从左往右逐个遍历，设置两个指针分别表示当前位置+1的值和数组末尾的值，使用 while 查找两个指针对应值的和等于-的当前下标值。

1. **排序：** 数组 `nums` 被按升序排序。排序是这个算法的关键步骤，因为它允许我们轻松地在数组中导航并处理重复元素。
2. **遍历数组：** 函数使用一个 for 循环遍历排序后的数组 `nums`。循环变量 `i` 表示三元组的第一个元素的潜在候选项。
3. **避免重复：** 在循环内部，有一个检查以跳过重复元素。如果当前元素与前一个元素相同，则继续下一次迭代，以避免重复的三元组。
4. **双指针法：** 对于每个 `nums[i]`，算法使用双指针法。两个指针 `low` 和 `high` 从剩余元素的两端开始。指针朝着彼此移动，以找到和为目标值（`-nums[i]`）的数对。
5. **检查目标和：** 检查当前三元组的和（`nums[i] + nums[low] + nums[high]`）是否等于目标值（`0`）。根据结果，调整指针的位置。
   * 如果和等于目标值，则找到一个有效的三元组，并将其添加到 `result` 数组中。
   * 如果和小于目标值，则增加 `low` 指针。
   * 如果和大于目标值，则减小 `high` 指针。
6. **避免重复的三元组：** 在找到有效的三元组之后，进行额外的检查和增量操作，以避免重复的三元组。这包括将 `low` 指针移过重复元素，并将 `high` 指针移至重复元素的前面。
7. **返回结果：** 函数返回一个包含所有和为0的不重复三元组的数组。

```typescript
function threeSum(nums: number[]): number[][] {
    // 存储结果的数组
    const result: number[][] = [];

    // 数组排序（升序）
    nums.sort((a, b) => a - b);

    // 遍历数组
    for (let i = 0; i < nums.length - 2; i++) {
        // 避免处理重复的元素
        if (i === 0 || (i > 0 && nums[i] !== nums[i - 1])) {
            // 初始化两个指针，分别指向当前元素后的数组的两端
            let low = i + 1;
            let high = nums.length - 1;
            // 计算目标值，使得三元组的和为0
            const target = -nums[i];

            // 使用双指针法寻找和为目标值的数对
            while (low < high) {
                // 计算当前三元组的和
                const sum = nums[low] + nums[high];

                // 判断和与目标值的关系
                if (sum === target) {
                    // 找到一个有效的三元组，添加到结果数组中
                    result.push([nums[i], nums[low], nums[high]]);
                    
                    // 优化：避免处理重复的元素，不会累计时间复杂度
                    while (low < high && nums[low] === nums[low + 1]) low++;
                    while (low < high && nums[high] === nums[high - 1]) high--;
                    
                    // 移动指针
                    low++;
                    high--;
                } else if (sum < target) {
                    // 当前和小于目标值，增加 low 指针
                    low++;
                } else {
                    // 当前和大于目标值，减小 high 指针
                    high--;
                }
            }
        }
    }

    // 返回最终结果数组
    return result;
}

```

**复杂度分析**

* 时间复杂度：$$O(N^2)$$&#x20;
* 空间复杂度：O(1)
