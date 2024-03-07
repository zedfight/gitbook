# 有效的括号

[**leetcode 题目链接**](https://leetcode.cn/problems/valid-parentheses/description/)

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

## 栈

**思路及算法**

使用一个栈（stack）来跟踪左括号，当遇到右括号时，它会检查栈顶的左括号是否匹配。如果匹配，则将栈顶的左括号弹出，继续检查下一个字符。最终，如果栈为空，说明所有括号都有匹配，函数返回 `true`，否则返回 `false`。

```typescript
function isValid(s: string): boolean {
    const stack: string[] = [];

    const isOpeningBracket = (char: string): boolean => {
        return ['(', '{', '['].includes(char);
    };

    const isMatchingPair = (opening: string, closing: string): boolean => {
        return (
            (opening === '(' && closing === ')') ||
            (opening === '{' && closing === '}') ||
            (opening === '[' && closing === ']')
        );
    };

    for (let i = 0; i < s.length; i++) {
        const char = s[i];

        if (isOpeningBracket(char)) {
            stack.push(char);
        } else {
            const lastOpeningBracket = stack.pop();
            if (!lastOpeningBracket || !isMatchingPair(lastOpeningBracket, char)) {
                return false;
            }
        }
    }

    // If the stack is empty, all brackets are matched
    return stack.length === 0;
}
```

**复杂度分析**

* 时间复杂度：O(n)，其中 n 是字符串 s 的长度。
* 空间复杂度：O(n)，栈中的字符数量为 O(n)。

