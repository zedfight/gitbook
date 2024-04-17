# 找到字符串中所有字母异位词

[**leetcode 题目链接**](https://leetcode.cn/problems/find-all-anagrams-in-a-string/description/)

给定两个字符串 `s` 和 `p`，找到 `s` 中所有 `p` 的 **异位词** 的子串，返回这些子串的起始索引。不考虑答案输出的顺序。

**异位词** 指由相同字母重排列形成的字符串（包括相同的字符串）。

**示例 1:**

<pre><code><strong>输入: s = "cbaebabacd", p = "abc"
</strong><strong>输出: [0,6]
</strong><strong>解释:
</strong>起始索引等于 0 的子串是 "cba", 它是 "abc" 的异位词。
起始索引等于 6 的子串是 "bac", 它是 "abc" 的异位词。
</code></pre>

&#x20;**示例 2:**

<pre><code><strong>输入: s = "abab", p = "ab"
</strong><strong>输出: [0,1,2]
</strong><strong>解释:
</strong>起始索引等于 0 的子串是 "ab", 它是 "ab" 的异位词。
起始索引等于 1 的子串是 "ba", 它是 "ab" 的异位词。
起始索引等于 2 的子串是 "ab", 它是 "ab" 的异位词。
</code></pre>

**提示:**

* `1 <= s.length, p.length <= 3 * 104`
* `s` 和 `p` 仅包含小写字母

## 滑动窗口

**思路及算法**

使用两个指针，右指针先向右滑动，同时记录字符，并在滑动距离等于异位词长度时检查是否为异位词。然后，左指针向右滑动一位，将右指针滑动过程中记录的字符还原。重复这个过程直到遍历整个字符串。注意count是记录是否匹配完异位词（也就是值为0时），只有在匹配完成时进行塞入数组。

1. **初始化字符计数器：** 使用 Map 数据结构初始化字符计数器 charCount，遍历字符串p，对每个字符进行计数。
2. **滑动窗口：** 使用两个指针 left 和 right 维护一个滑动窗口，初始时窗口大小为0。right 指针向右移动，同时更新字符计数器。
3. **更新字符计数器：** 如果右指针指向的字符在字符计数器中，将其计数减一。
4. **检查窗口大小：** 当窗口大小等于字符串p的长度时，检查是否为异位词。如果 count 等于0，表示窗口内的字符正好匹配字符串p中的所有字符，将 left 指针的位置加入结果数组。
5. **移动左指针：** 移动左指针 left，同时恢复字符计数器。如果左指针指向的字符在字符计数器中，将其计数加一。
6. **重复：** 重复步骤2-5，直到右指针遍历完整个字符串s。

```typescript
function findAnagrams(s: string, p: string): number[] {
    const result: number[] = [];
    const charCount: Map<string, number> = new Map();

    // 初始化字符计数器
    for (const char of p) {
        charCount.set(char, (charCount.get(char) || 0) + 1);
    }

    let left = 0;
    let right = 0;
    let count = p.length;

    while (right < s.length) {
        // 移动右指针，更新字符计数器
        if (charCount.has(s[right])) {
            // charCount 初始状态从 1 开始，如果匹配了一个字符则 count 需要减 1，如果 charCount 小于 1（初始值）时就算匹配到了也不需要再减 1
            // 为什么小于 1 就不需要减 1？说明当前字符已经被匹配完了，如果再次碰到以匹配的字符需要跳过当前逻辑
            if (charCount.get(s[right])! >= 1) {
                count--;
            }
          
            charCount.set(s[right], charCount.get(s[right])! - 1);
        }
      
        right++;
        
        // 当窗口大小等于p的长度时，检查是否为异位词
        if (right - left === p.length) {
            if (count === 0) {
                result.push(left);
            }

            // 移动左指针，恢复字符计数器
            if (charCount.has(s[left])) {
                charCount.set(s[left], charCount.get(s[left])! + 1);
              
                // 因为 count 只有在 charCount s[right] >= 1 时自减，所以当 charCount s[left] 数量加 1 后必须在 >= 1 的时候进行自增
                if (charCount.get(s[left])! >= 1) {
                    count++;
                }
            }

            left++;
        }
    }

    return result;
}
```

**复杂度分析**

* 时间复杂度：$$O(N)$$,N表示S的长度
* 空间复杂度：$$O(N)$$,N表示S的长度
