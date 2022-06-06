# 洗牌

- [算法之洗牌算法（随机置乱算法） ](https://www.cnblogs.com/z-sm/p/12393211.html)
- [三种洗牌算法shuffle](https://blog.csdn.net/qq_26399665/article/details/79831490)
- [【算法】扑克随机洗牌算法分析](https://zhuanlan.zhihu.com/p/135268676)
- [Fisher–Yates shuffle 洗牌算法](https://gaohaoyang.github.io/2016/10/16/shuffle-algorithm/)

```js
const arr = [1, 2, 3];

const shuffleArr = (arr: number[]) => arr.sort(() => 0.5 - Math.random());

console.log(shuffleArr(arr)); // [3, 1, 2]
```
