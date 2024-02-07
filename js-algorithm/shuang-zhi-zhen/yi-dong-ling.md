# 移动零

[**leetcode题目链接**](https://leetcode.cn/problems/move-zeroes/description/)

给定一个数组 `nums`，编写一个函数将所有 `0` 移动到数组的末尾，同时保持非零元素的相对顺序。

**请注意** ，必须在不复制数组的情况下原地对数组进行操作。

**示例 1:**

<pre><code><strong>输入: nums = [0,1,0,3,12]
</strong><strong>输出: [1,3,12,0,0]
</strong></code></pre>

**示例 2:**

<pre><code><strong>输入: nums = [0]
</strong><strong>输出: [0]
</strong></code></pre>

**提示**:

* `1 <= nums.length <= 104`
* `-231 <= nums[i] <= 231 - 1`

## 暴力算法

**思路及算法**

1. 使用 `while` 循环来遍历数组，通过变量 `i` 控制当前处理的数组元素的索引。
2. 在循环中，检查当前元素 `nums[i]` 是否为零。
   * 如果 `nums[i]` 为零，使用 `splice(i, 1)` 删除数组中的该零元素。
   * 然后，使用 `nums.push(0)` 将零元素添加到数组的末尾。
   * 同时，减小 `length` 变量，以便减少循环次数，因为数组末尾已经有一个零元素。
3. 如果 `nums[i]` 不为零，则递增 `i`，继续下一个元素的处理。
4. 重复以上步骤，直到遍历完整个数组。

```typescript
function moveZeroes1(nums: number[]): void {
    let length: number = nums.length;
    let i = 0;
    while (i < length) {
        if (nums[i] === 0) {
            nums.splice(i, 1);
            nums.push(0);
            length--;
        } else {
            i++;
        }
    }
}
```

**复杂度分析**

* 时间复杂度：$$O(N^2)$$
* 空间复杂度：$$O(1)$$

## 单指针

**思路及算法**

`i` 用于遍历数组，`nonZeroIndex` 用于记录非零元素的位置。首先，我们遍历数组，将非零元素移动到数组的前面。然后，我们将数组剩余部分填充为零

```typescript
function moveZeroes(nums: number[]): void {
    let nonZeroIndex = 0;

    // 将非零元素移动到数组的前面
    for (let i = 0; i < nums.length; i++) {
        if (nums[i] !== 0) {
            nums[nonZeroIndex] = nums[i];
            nonZeroIndex++;
        }
    }

    // 将数组剩余部分填充为零
    for (let i = nonZeroIndex; i < nums.length; i++) {
        nums[i] = 0;
    }
}
```

**复杂度分析**

* 时间复杂度：$$O(N)$$
* 空间复杂度：$$O(1)$$

## 双指针

**思路及算法**

1. 初始化两个指针 `left` 和 `right`，都指向数组的起始位置，即0。
2. 使用 `while` 循环，遍历数组直到 `right` 指针到达数组末尾。
3. 在循环中，检查 `nums[right]` 是否为零。
   * 如果不为零，交换 `nums[left]` 和 `nums[right]` 的值，然后将 `left` 指针向右移动一步，确保非零元素被移到数组的左侧。
   * 如果为零，不进行交换，继续遍历。
4. 无论是否进行了交换，都将 `right` 指针向右移动一步。
5. 重复步骤 3-4 直到 `right` 指针到达数组末尾。

```typescript
function moveZeroes(nums: number[]): void {
    // nums[left] 和 nums[right] 之间要么相邻要么间隔 n 个 0
    // nums[left] 一定是 0，nums[right] 值一定不是 0
    let left = 0;
    let right = 0;

    while (right < nums.length) {
        if (nums[right] !== 0) {
            // 交换 left 和 right 的值，nums[right] 赋值给 nums[left], nums[left] 赋值给 nums[right]
            // 交换非零元素到数组的左侧
            [nums[left], nums[right]] = [nums[right], nums[left]];
            left++;
        }

        right++;
    }
}
```

**复杂度分析**

* 时间复杂度：$$O(N)$$
* 空间复杂度：$$O(1)$$

## 排序

**思路及算法**

1. 如果元素 `a` 是零而元素 `b` 不是零，则将 `a` 排在 `b` 之后（返回1）。
2. 如果元素 `a` 不是零而元素 `b` 是零，则将 `a` 排在 `b` 之前（返回-1）。
3. 如果两个元素都是零或者都不是零，则它们之间的相对顺序不变（返回0）。

```typescript
function moveZeroes(nums: number[]): void {
    nums.sort((a, b) => {
        if (a === 0 && b !== 0) {
            return 1;
        } else if (a !== 0 && b === 0) {
            return -1;
        } else {
            return 0;
        }
    });
};
```

**复杂度分析**

* 时间复杂度：O(nlogn)
* 空间复杂度：O(1)
