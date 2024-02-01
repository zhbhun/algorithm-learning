# 两数之和

## 问题

给定一个整数数组 nums 和一个整数目标值 target，请你在该数组中找出和为目标值 target 的那两个整数，并返回它们的数组下标。

你可以假设每种输入只会对应一个答案。但是，数组中同一个元素在答案里不能重复出现。

你可以按任意顺序返回答案。

示例 1：

```
输入：nums = [2,7,11,15], target = 9
输出：[0,1]
解释：因为 nums[0] + nums[1] == 9 ，返回 [0, 1] 。
```

示例 2：

```
输入：nums = [3,2,4], target = 6
输出：[1,2]

示例 3：

输入：nums = [3,3], target = 6
输出：[0,1]
```

提示：

- `2 <= nums.length <= 104`
- `-109 <= nums[i] <= 109`
- `-109 <= target <= 109`
- 只会存在一个有效答案

进阶：你可以想出一个时间复杂度小于 O(n2) 的算法吗？

## 解答

考核 Hash 表的使用。

### 暴力枚举

```js
function twoSum(nums, target) {
  const n = nums.length
  for (let i = 0; i < n; ++i) {
    for (let j = i + 1; j < n; ++j) {
      if (nums[i] + nums[j] == target) {
        return [i, j]
      }
    }
  }
  return new int[0]()
}
```

复杂度分析

- 时间复杂度：$O(n^2)$，其中 N 是数组中的元素数量。最坏情况下数组中任意两个数都要被匹配一次。
- 空间复杂度：$O(1)$。

### 哈希表

```js
function twoSum(nums, target) {
  const hashtable = {}
  for (let i = 0; i < nums.length; ++i) {
    hashtable[nums[i]] = i
  }
  for (let i = 0; i < nums.length; ++i) {
    const numValue = nums[i]
    const anotherNumIndex = hashtable[target - nums[i]]
    if (i !== anotherNumIndex && anotherNumIndex >= 0) {
      return [anotherNumIndex, i]
    }
  }
  return [0]
}
```

复杂度分析

- 时间复杂度：$O(n)$，其中 N 是数组中的元素数量。对于每一个元素 x，我们可以 O(1) 地寻找 target - x。
- 空间复杂度：$O(n)$，其中 N 是数组中的元素数量。主要为哈希表的开销。

## 参考文献

- https://leetcode.cn/problems/two-sum/
