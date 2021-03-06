# 面试题49. 丑数

我们把只包含因子 2、3 和 5 的数称作丑数（Ugly Number）。求按从小到大的顺序的第 n 个丑数。

 

示例:

输入: n = 10
输出: 12
解释: 1, 2, 3, 4, 5, 6, 8, 9, 10, 12 是前 10 个丑数。
说明:  

1 是丑数。
n 不超过1690。
注意：本题与主站 264 题相同：https://leetcode-cn.com/problems/ugly-number-ii/

通过次数7,052提交次数11,250

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/chou-shu-lcof

## 1.动态规划

```js
/**
 * 动态规划
 * dp[i]: 第i个丑数
 * 最优子结构: 左前最靠近i的因子为2的丑数dp[a], 左前最靠近i的因子为3的丑数dp[b]，左前最靠近i的因子为5的丑数dp[c]
 * dp[i]是丑数，所以肯定是dp[c] * 2或dp[b] * 2或dp[c] * 5得到，当前是要找下一个丑数，所以肯定是取前三者最小之一
 * 状态转移方程：dp[i] = Math.min(dp[i-1] * 2, dp[i-1] * 3. dp[i-1] * 5)
 * @param {number} n
 * @return {number}
 */
var nthUglyNumber = function(n) {
  let ugly1 = 1;
  let ugly2 = 1;
  let ugly3 = 1;
  let dp = [0, 1];
  let count = 2;
  while (count <= n) {
    let tmp1 = dp[ugly1] * 2;
    let tmp2 = dp[ugly2] * 3;
    let tmp3 = dp[ugly3] * 5;
    dp[count] = Math.min(tmp1, tmp2, tmp3);
    if (dp[count] === tmp1) ugly1++;
    if (dp[count] === tmp2) ugly2++;
    if (dp[count] === tmp3) ugly3++;
    count++;
  }
  return dp[n];
};
```