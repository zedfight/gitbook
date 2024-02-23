# 无重复字符的最长子串

[**leetcode 题目链接**](https://leetcode.cn/problems/longest-substring-without-repeating-characters/)

给定一个字符串 `s` ，请你找出其中不含有重复字符的 **最长子串** 的长度。

**示例 1:**

<pre><code><strong>输入: s = "abcabcbb"
</strong><strong>输出: 3 
</strong><strong>解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。
</strong></code></pre>

**示例 2:**

<pre><code><strong>输入: s = "bbbbb"
</strong><strong>输出: 1
</strong><strong>解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。
</strong></code></pre>

**示例 3:**

<pre><code><strong>输入: s = "pwwkew"
</strong><strong>输出: 3
</strong><strong>解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。
</strong><strong>     请注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串。
</strong></code></pre>

**提示：**

* `0 <= s.length <= 5 * 104`
* `s` 由英文字母、数字、符号和空格组成

## 滑动窗口

**思路及算法**

1. 初始化滑动窗口的起始和结束索引。
2. 使用一个 Set 数据结构来存储当前窗口内的字符。
3. 移动窗口的结束索引，直到遇到重复字符。
4. 移动窗口的起始索引，直到删除重复字符，确保窗口内没有重复字符。
5. 在整个过程中，保持记录窗口的最大长度。

```typescript
function lengthOfLongestSubstring(s: string): number {
    let left = 0; // 滑动窗口的起始索引
    let right = 0; // 滑动窗口的结束索引
    let maxLength = 0; // 记录最大长度
    const charSet = new Set(); // 存储当前窗口内的字符

    while (right < s.length) {
        if (!charSet.has(s[right])) {
            // 当前字符不在窗口内，扩展窗口
            charSet.add(s[right]);
            maxLength = Math.max(maxLength, right - left + 1);
            right++;
        } else {
            // 当前字符已经在窗口内，缩小窗口
            charSet.delete(s[left]);
            left++;
        }
    }

    return maxLength;
}
```

**复杂度分析**

* 时间复杂度：$$O(N)$$
* 空间复杂度：O(1)，字符的 ASCII 码范围为 0 \~ 127 ，哈希表 dicdicdic 最多使用 O(128) = O(1)大小的额外空间，也可以表示为O(∣Σ∣)，∣Σ∣ = 128。
