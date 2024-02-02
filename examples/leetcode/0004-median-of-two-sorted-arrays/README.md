# 寻找两个正序数组的中位数

寻找两个正序数组的中位数。

## 题目

> 给定两个大小为 m 和 n 的正序（从小到大）数组 nums1 和 nums2。
请你找出这两个正序数组的中位数，并且要求算法的时间复杂度为 O(log(m + n))。
你可以假设 nums1 和 nums2 不会同时为空。

```js
示例1：
nums1 = [1, 3]
nums2 = [2]
    
则中位数是 2.0
    
示例2：
nums1 = [1, 2]
nums2 = [3, 4]
    
则中位数是 (2 + 3) / 2 = 2.5
```

## 解答

这道题网络上的解析都非常“高深”，很难理解。私以为它们都将简单的问题复杂化了。本题在一些处理上确实会有些麻烦，比如数组边界的处理，和偶数个数的中位数的处理。但其核心思想并不复杂。


### 代码实现

Java语言

```java
public class Solution {
  public double findMedianSortedArrays(int[] nums1, int[] nums2) {
    // 使nums1成为较短数组,不仅可以提高检索速度,同时可以避免一些边界问题
    if (nums1.length > nums2.length) {
      int[] temp = nums1;
      nums1 = nums2;
      nums2 = temp;
    }

    int len1 = nums1.length;
    int len2 = nums2.length;
    int leftLen = (len1 + len2 + 1) / 2; //两数组合并&排序后,左半边的长度
    
    // 对数组1进行二分检索
    int start = 0;
    int end = len1;
    while (start <= end) {
      // 两个数组的被测数A,B的位置(从1开始计算)
      // count1 = 2 表示 num1 数组的第2个数字
      // 比index大1
      int count1 = start + ((end - start) / 2);
      int count2 = leftLen - count1;
      
      if (count1 > 0 && nums1[count1 - 1] > nums2[count2]) {
        // A比B的next还要大
        end = count1 - 1;
      } else if (count1 < len1 && nums2[count2 - 1] > nums1[count1]) {
        // B比A的next还要大
        start = count1 + 1;
      } else {
        // 获取中位数
        int result =  (count1 == 0)? nums2[count2 - 1]: // 当num1数组的数都在总数组右边
                      (count2 == 0)? nums1[count1 - 1]: // 当num2数组的数都在总数组右边
                      Math.max(nums1[count1 - 1], nums2[count2 - 1]); // 比较A,B
        if (isOdd(len1 + len2)) {
          return result;
        }

        // 处理偶数个数的情况
        int nextValue = (count1 == len1) ? nums2[count2]:
                        (count2 == len2) ? nums1[count1]:
                        Math.min(nums1[count1], nums2[count2]);
        return (result + nextValue) / 2.0;
      }
    }

    return Integer.MIN_VALUE; // 绝对到不了这里
  }

  // 奇数返回true,偶数返回false
  private boolean isOdd(int x) {
    return (x & 1) == 1;
  }
}
```

#### 复杂度分析

- 时间复杂度：对数组进行二分查找，因此为O(logN)
- 空间复杂度：O(1)

