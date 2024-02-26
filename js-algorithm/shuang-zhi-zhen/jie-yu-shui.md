# 接雨水

[**leetcode题目链接**](https://leetcode.cn/problems/trapping-rain-water/description/)

给定 `n` 个非负整数表示每个宽度为 `1` 的柱子的高度图，计算按此排列的柱子，下雨之后能接多少雨水。

**示例 1：**

![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/10/22/rainwatertrap.png)

<pre><code><strong>输入：height = [0,1,0,2,1,0,1,3,2,1,2,1]
</strong><strong>输出：6
</strong><strong>解释：上面是由数组 [0,1,0,2,1,0,1,3,2,1,2,1] 表示的高度图，在这种情况下，可以接 6 个单位的雨水（蓝色部分表示雨水）。 
</strong></code></pre>

**示例 2：**

<pre><code><strong>输入：height = [4,2,0,3,2,5]
</strong><strong>输出：9
</strong></code></pre>

**提示：**

* `n == height.length`
* `1 <= n <= 2 * 104`
* `0 <= height[i] <= 105`

## 动态规划

**思路及算法**

TODO

**复杂度分析**

## 单调栈

TODO

**思路及算法**

**复杂度分析**

## 双指针

**思路及算法**

1. 初始化变量：`ans` 用于存储结果，`left` 和 `right` 作为数组两端的指针，`preMax` 和 `sufMax` 用于追踪从左侧和右侧遇到的最大高度。
2. 使用 while 循环遍历数组，直到 `left` 大于等于 `right`。
3. 在循环中：
   * 更新 `preMax` 和 `sufMax`，分别存储从左侧和右侧遇到的最大高度。
   * 比较 `preMax` 和 `sufMax`。如果 `preMax` 小于 `sufMax`，意味着较小的一侧确定了当前的积水高度。将 `ans` 增加 `preMax` 与 `left` 处高度的差值，并将 `left` 指针向右移动。
   * 如果 `sufMax` 小于或等于 `preMax`，将 `ans` 增加 `sufMax` 与 `right` 处高度的差值，并将 `right` 指针向左移动。
4. 持续此过程直到 `left` 不再小于 `right`。
5. 返回存储在 `ans` 中的最终结果。

{% tabs %}
{% tab title="1" %}
<figure><img src="../.gitbook/assets/image (3) (1).png" alt=""><figcaption><p>1</p></figcaption></figure>
{% endtab %}

{% tab title="2" %}
<figure><img src="../.gitbook/assets/image (5) (1).png" alt=""><figcaption><p>2</p></figcaption></figure>
{% endtab %}

{% tab title="3" %}
<figure><img src="../.gitbook/assets/image (6) (1).png" alt=""><figcaption><p>3</p></figcaption></figure>
{% endtab %}

{% tab title="4" %}
<figure><img src="../.gitbook/assets/image (7) (1).png" alt=""><figcaption><p>4</p></figcaption></figure>
{% endtab %}

{% tab title="5" %}
<figure><img src="../.gitbook/assets/image (8) (1).png" alt=""><figcaption><p>5</p></figcaption></figure>
{% endtab %}

{% tab title="6" %}
<figure><img src="../.gitbook/assets/image (9) (1).png" alt=""><figcaption><p>6</p></figcaption></figure>
{% endtab %}

{% tab title="7" %}
<figure><img src="../.gitbook/assets/image (10).png" alt=""><figcaption><p>7</p></figcaption></figure>
{% endtab %}

{% tab title="8" %}
<figure><img src="../.gitbook/assets/image (11).png" alt=""><figcaption><p>8</p></figcaption></figure>
{% endtab %}

{% tab title="9" %}
<figure><img src="../.gitbook/assets/image (12).png" alt=""><figcaption><p>9</p></figcaption></figure>
{% endtab %}

{% tab title="10" %}
<figure><img src="../.gitbook/assets/image (13).png" alt=""><figcaption><p>10</p></figcaption></figure>
{% endtab %}

{% tab title="11" %}
<figure><img src="../.gitbook/assets/image (14).png" alt=""><figcaption><p>11</p></figcaption></figure>
{% endtab %}

{% tab title="12" %}
<figure><img src="../.gitbook/assets/image (15).png" alt=""><figcaption><p>12</p></figcaption></figure>
{% endtab %}
{% endtabs %}

```javascript
var trap = function (height) {
    let ans = 0, left = 0, right = height.length - 1, preMax = 0, sufMax = 0;
    while (left < right) {
        preMax = Math.max(preMax, height[left]);
        sufMax = Math.max(sufMax, height[right]);
        if (preMax < sufMax) {
            ans += preMax - height[left++];
        } else {
            ans += sufMax - height[right--];
        }
    }
    return ans; 
｝
```

**复杂度分析**

时间复杂度：O(n)

空间复杂度：O(1)
