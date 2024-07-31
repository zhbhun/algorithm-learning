# Z 字形变换


## 题目

将一个给定字符串 s 根据给定的行数 numRows ，以从上往下、从左到右进行 Z 字形排列。

比如输入字符串为 "PAYPALISHIRING" 行数为 3 时，排列如下：

```
P   A   H   N
A P L S I I G
Y   I   R
```

之后，你的输出需要从左往右逐行读取，产生出一个新的字符串，比如："PAHNAPLSIIGYIR"。

请你实现这个将字符串进行指定行数变换的函数：

string convert(string s, int numRows);
 

示例 1：

```
输入：s = "PAYPALISHIRING", numRows = 3
输出："PAHNAPLSIIGYIR"
```

示例 2：

```
输入：s = "PAYPALISHIRING", numRows = 4
输出："PINALSIGYAHRPI"
解释：
P     I    N
A   L S  I G
Y A   H R
P     I
```

示例 3：

```
输入：s = "A", numRows = 1
输出："A"
```

提示：

- 1 <= s.length <= 1000
- s 由英文字母（小写和大写）、',' 和 '.' 组成
- 1 <= numRows <= 1000


## 解答

考核矩阵和数据遍历规律的应用。

### 直接构造

```js
var convert = function (s, numRows) {
  const n = s.length,
    r = numRows;
  if (r === 1 || r >= n) {
    return s;
  }
  const t = r * 2 - 2;
  const ans = [];
  for (let i = 0; i < r; i++) {
    // 枚举矩阵的行
    for (let j = 0; j < n - i; j += t) {
      // 枚举每个周期的起始下标
      ans.push(s[j + i]); // 当前周期的第一个字符
      if (0 < i && i < r - 1 && j + t - i < n) {
        ans.push(s[j + t - i]); // 当前周期的第二个字符
      }
    }
  }
  return ans.join("");
};
```

复杂度分析

- 时间复杂度：$O(n)$，其中 nnn 为字符串 sss 的长度。sss 中的每个字符仅会被访问一次，因此时间复杂度为 O(n)O(n)O(n)。
- 空间复杂度：$O(1)$，返回值不计入空间复杂度。

### 利用二维矩阵模拟

```js
var convert = function (s, numRows) {
  const n = s.length,
    r = numRows;
  if (r === 1 || r >= n) {
    return s;
  }
  const t = r * 2 - 2;
  const c = Math.floor((n + t - 1) / t) * (r - 1);
  const mat = new Array(r).fill(0).map(() => new Array(c).fill(0));
  for (let i = 0, x = 0, y = 0; i < n; ++i) {
    mat[x][y] = s[i];
    if (i % t < r - 1) {
      ++x; // 向下移动
    } else {
      --x;
      ++y; // 向右上移动
    }
  }
  const ans = [];
  for (const row of mat) {
    for (const ch of row) {
      if (ch !== 0) {
        ans.push(ch);
      }
    }
  }
  return ans.join("");
};
```

复杂度分析

- 时间复杂度：$O(r*n)$，其中 r=numRows，n 为字符串 s 的长度。时间主要消耗在矩阵的创建和遍历上，矩阵的行数为 r，列数可以视为 $O(n)$。
- 空间复杂度：$O(r*n)$。
