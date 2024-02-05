# 两数之和

[**leetcode 题目链接**](https://leetcode.cn/problems/two-sum/description/)

给定一个整数数组 `nums` 和一个整数目标值 `target`，请你在该数组中找出**和为目标值** _`target`_  的那**两个**整数，并返回它们的数组下标。

假设每种输入只会对应一个答案。数组中同一个元素在答案里不能重复出现。返回答案可以任意顺序。

**示例 1：**

<pre><code><strong>输入：nums = [2,7,11,15], target = 9
</strong><strong>输出：[0,1]
</strong><strong>解释：因为 nums[0] + nums[1] == 9 ，返回 [0, 1] 。
</strong></code></pre>

**示例 2：**

<pre><code><strong>输入：nums = [3,2,4], target = 6
</strong><strong>输出：[1,2]
</strong></code></pre>

**示例 3：**

<pre><code><strong>输入：nums = [3,3], target = 6
</strong><strong>输出：[0,1]
</strong></code></pre>

**提示：**

* `2 <= nums.length <= 104`
* `-109 <= nums[i] <= 109`
* `-109 <= target <= 109`
* **只存在一个有效答案**

## 暴力枚举

**思路及算法**

最直观的方法是使用**暴力枚举**方式，我们可以遍历每个元素 `x`，并查找是否存在一个值等于 `target - x` 的元素。这样我们可以检查数组中的每对元素，看它们的和是否等于目标值。

```typescript
class Solution {
    public int[] twoSum(int[] nums, int target) {
        int n = nums.length;
        for (int i = 0; i < n; ++i) {
            for (int j = i + 1; j < n; ++j) {
                if (nums[i] + nums[j] == target) {
                    return new int[]{i, j};
                }
            }
        }
        return new int[0];
    }
}
```

**复杂度分析**

* 时间复杂度：$$O(N^2)$$，其中 $$N$$ 是数组中的元素数量。
* 空间复杂度：O(1)

#### 解法二：哈希表

## 哈希表

**思路及算法**

为了提高效率，我们可以使用**哈希表**来优化查找过程。具体思路如下：

1. 遍历数组 `nums`。
2. 对每一个元素 `x`，在哈希表中查找 `target - x` 是否存在。
3. 如果存在，则直接返回两个元素的下标。
4. 如果不存在，则将当前元素 `x` 及其下标存入哈希表。

通过哈希表，我们可以在 O(1) 的时间复杂度内查找某个元素是否存在。

```typescript
class Solution {
    public int[] twoSum(int[] nums, int target) {
        Map<Integer, Integer> hashtable = new HashMap<Integer, Integer>();
        for (int i = 0; i < nums.length; ++i) {
            if (hashtable.containsKey(target - nums[i])) {
                return new int[]{hashtable.get(target - nums[i]), i};
            }
            hashtable.put(nums[i], i);
        }
        return new int[0];
    }
}
```

**复杂度分析**

时间复杂度：O(N)

空间复杂度：O(N)
