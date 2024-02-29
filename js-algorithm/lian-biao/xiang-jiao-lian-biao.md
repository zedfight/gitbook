# 相交链表

[**leetcode 题目链接**](https://leetcode.cn/problems/intersection-of-two-linked-lists/description/)

给你两个单链表的头节点 `headA` 和 `headB` ，请你找出并返回两个单链表相交的起始节点。如果两个链表不存在相交节点，返回 `null` 。

图示两个链表在节点 `c1` 开始相交**：**

[![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/160\_statement.png)](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/160\_statement.png)

题目数据 **保证** 整个链式结构中不存在环。

**注意**，函数返回结果后，链表必须 **保持其原始结构** 。

**自定义评测：**

**评测系统** 的输入如下（你设计的程序 **不适用** 此输入）：

* `intersectVal` - 相交的起始节点的值。如果不存在相交节点，这一值为 `0`
* `listA` - 第一个链表
* `listB` - 第二个链表
* `skipA` - 在 `listA` 中（从头节点开始）跳到交叉节点的节点数
* `skipB` - 在 `listB` 中（从头节点开始）跳到交叉节点的节点数

评测系统将根据这些输入创建链式数据结构，并将两个头节点 `headA` 和 `headB` 传递给你的程序。如果程序能够正确返回相交节点，那么你的解决方案将被 **视作正确答案** 。

**示例 1：**

[![](https://assets.leetcode.com/uploads/2021/03/05/160\_example\_1\_1.png)](https://assets.leetcode.com/uploads/2018/12/13/160\_example\_1.png)

<pre><code><strong>输入：intersectVal = 8, listA = [4,1,8,4,5], listB = [5,6,1,8,4,5], skipA = 2, skipB = 3
</strong><strong>输出：Intersected at '8'
</strong><strong>解释：相交节点的值为 8 （注意，如果两个链表相交则不能为 0）。
</strong>从各自的表头开始算起，链表 A 为 [4,1,8,4,5]，链表 B 为 [5,6,1,8,4,5]。
在 A 中，相交节点前有 2 个节点；在 B 中，相交节点前有 3 个节点。
— 请注意相交节点的值不为 1，因为在链表 A 和链表 B 之中值为 1 的节点 (A 中第二个节点和 B 中第三个节点) 是不同的节点。换句话说，它们在内存中指向两个不同的位置，而链表 A 和链表 B 中值为 8 的节点 (A 中第三个节点，B 中第四个节点) 在内存中指向相同的位置。
why??？为什么不是 1 而是 8 ？？因为图中所示才是内存存储情况，可以看出 1 值虽然相同但是内存地址不同，8 值相同且内存地址也相同。
</code></pre>

**示例 2：**

[![](https://assets.leetcode.com/uploads/2021/03/05/160\_example\_2.png)](https://assets.leetcode.com/uploads/2018/12/13/160\_example\_2.png)

<pre><code><strong>输入：intersectVal = 2, listA = [1,9,1,2,4], listB = [3,2,4], skipA = 3, skipB = 1
</strong><strong>输出：Intersected at '2'
</strong><strong>解释：相交节点的值为 2 （注意，如果两个链表相交则不能为 0）。
</strong>从各自的表头开始算起，链表 A 为 [1,9,1,2,4]，链表 B 为 [3,2,4]。
在 A 中，相交节点前有 3 个节点；在 B 中，相交节点前有 1 个节点。
</code></pre>

**示例 3：**

[![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/160\_example\_3.png)](https://assets.leetcode.com/uploads/2018/12/13/160\_example\_3.png)

<pre><code><strong>输入：intersectVal = 0, listA = [2,6,4], listB = [1,5], skipA = 3, skipB = 2
</strong><strong>输出：null
</strong><strong>解释：从各自的表头开始算起，链表 A 为 [2,6,4]，链表 B 为 [1,5]。
</strong>由于这两个链表不相交，所以 intersectVal 必须为 0，而 skipA 和 skipB 可以是任意值。
这两个链表不相交，因此返回 null 。
</code></pre>

**提示：**

* `listA` 中节点数目为 `m`
* `listB` 中节点数目为 `n`
* `1 <= m, n <= 3 * 104`
* `1 <= Node.val <= 105`
* `0 <= skipA <= m`
* `0 <= skipB <= n`
* 如果 `listA` 和 `listB` 没有交点，`intersectVal` 为 `0`
* 如果 `listA` 和 `listB` 有交点，`intersectVal == listA[skipA] == listB[skipB]`

**进阶：**你能否设计一个时间复杂度 `O(m + n)` 、仅用 `O(1)` 内存的解决方案？

## 双指针

**思路及算法**

```haskell
pA:1->2->3->4->5->6->null->9->5->6->null
pB:9->5->6->null->1->2->3->4->5->6->null
```

1. 初始化两个指针 `pointerA` 和 `pointerB` 分别指向链表 `headA` 和 `headB` 的头节点。
2. 使用一个循环，判断 `pointerA` 和 `pointerB` 是否相等。如果相等，说明找到了相交点，返回相交节点；如果不相等，继续执行下面的步骤。
3. 在循环内，判断 `pointerA` 是否为 null。如果是，将其重定向到链表 `headB` 的头部；否则，将 `pointerA` 移动到下一个节点。
4. 同样，在循环内，判断 `pointerB` 是否为 null。如果是，将其重定向到链表 `headA` 的头部；否则，将 `pointerB` 移动到下一个节点。
5. 重复步骤 2-4 直到找到相交点或者同时到达链表末尾（没有相交点）。

```typescript
class ListNode {
    val: number;
    next: ListNode | null;
    constructor(val: number) {
        this.val = val;
        this.next = null;
    }
}

function getIntersectionNode(headA: ListNode | null, headB: ListNode | null): ListNode | null {
    if (!headA || !headB) {
        return null; // 如果其中一个链表为空，则不存在相交节点
    }

    let pointerA: ListNode | null = headA;
    let pointerB: ListNode | null = headB;

    while (pointerA !== pointerB) {
        // 如果当前指针到达链表末尾，则重定向到另一个链表的头部
        pointerA = pointerA ? pointerA.next : headB;
        pointerB = pointerB ? pointerB.next : headA;
    }

    return pointerA; // 返回相交节点或 null
}
```

**复杂度分析**

* 时间复杂度：$$O(m + n)$$，其中 m 和 n 分别是两个链表的长度。
* 空间复杂度：$$O(1)$$
