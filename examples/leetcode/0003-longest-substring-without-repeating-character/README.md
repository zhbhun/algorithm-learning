# 无重复字符的最长子串

## 问退

给定一个字符串 s，请你找出其中不含有重复字符的 最长子串 的长度。

- 示例 1:

    ```
    输入: s = "abcabcbb"
    输出: 3 
    解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。
    ```

- 示例 2：

    ```
    输入: s = "bbbbb"
    输出: 1
    解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。
    ```

- 示例 3：

    ```
    输入: s = "pwwkew"
    输出: 3
    解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。
         请注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串。
    ```

## 参考

https://leetcode.cn/problems/longest-substring-without-repeating-characters

## 解答

### 滑动窗口

1. 示例：以示例一中的字符串 abcabcbb 为例，我们列举出这些结果，其中括号中表示选中的字符以及最长的字符串：

    - 以 (a)bcabcbb 开始的最长字符串为 (abc)abcbb；
    - 以 (a)bcabcbb 开始的最长字符串为 (abc)abcbb;
    - 以 a(b)cabcbb 开始的最长字符串为a(bca)bcbb;
    - 以 ab(c)abcbb 开始的最长字符串为 ab(cab)cbb;
    - 以 abc(a)bcbb 开始的最长字符串为 abc(abc)bb;
    - 以 abca(b)cbb 开始的最长字符串为 abca (bc)bb;
    - 以 abcab(c)bb 开始的最长字符串为 abcab (cb)b；
    - 以 abcabc(b)b 开始的最长字符串为 abcabc (b)b;
    - 以 abcabcb(b）开始的最长字符串为 abcabcb(b)。

2. 规律：假设我们选择宇符串中的第 k 个宇符作为起始位置，并且得到了不包含重复字符的最长子串的结束位置为 r(k)。那么当我们选择第 k＋1 个字符作为起始位置时，首先从 k十1 到 r(k) 的字符显然是不重复的，并且由于少了原本的第 k 个字符，我们可以尝试继续增大 r(k)，直到右侧出现了重复字符为止。
3. 解决：

    ```js
    var lengthOfLongestSubstring = function(s) {
        // 哈希集合，记录每个字符是否出现过
        const occ = new Set();
        const n = s.length;
        // 右指针，初始值为 -1，相当于我们在字符串的左边界的左侧，还没有开始移动
        let rk = -1, ans = 0;
        for (let i = 0; i < n; ++i) {
            if (i != 0) {
                // 左指针向右移动一格，移除一个字符
                occ.delete(s.charAt(i - 1));
            }
            while (rk + 1 < n && !occ.has(s.charAt(rk + 1))) {
                // 不断地移动右指针
                occ.add(s.charAt(rk + 1));
                ++rk;
            }
            // 第 i 到 rk 个字符是一个极长的无重复字符子串
            ans = Math.max(ans, rk - i + 1);
        }
        return ans;
    };
    ```

4. 复杂度

    - 时间复杂度：$O(N)$，，其中 N 是字符串的长度。左指针和右指针分别会遍历整个字符串一次。
    - 空间复杂度：$O(|Σ|)$，其中 Σ 表示字符集（即字符串中可以出现的字符），∣Σ∣ 表示字符集的大小。

## 总结

考核集合与遍历。
