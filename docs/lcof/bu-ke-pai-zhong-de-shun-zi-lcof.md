# 面试题61. 扑克牌中的顺子

从扑克牌中随机抽5张牌，判断是不是一个顺子，即这5张牌是不是连续的。2～10为数字本身，A为1，J为11，Q为12，K为13，而大、小王为 0 ，可以看成任意数字。A 不能视为 14。

 

示例 1:

输入: [1,2,3,4,5]
输出: True
 

示例 2:

输入: [0,0,1,2,5]
输出: True
 

限制：

数组长度为 5 

数组的数取值为 [0, 13] .

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/bu-ke-pai-zhong-de-shun-zi-lcof

## 1.找最大最小值

```js
/**
 * 1.连续序列的特点：max - min + 1 = 序列长度，由于0可以变化，所以max - min + 1 <- 序列长度
 * 2.如果存在重复，则不是顺子
 * @param {number[]} nums
 * @return {boolean}
 */
var isStraight = function(nums) {
  let min = 14
  let max = -1;
  const repeated = [];
  for (let i = 0; i < nums.length; i++) {
    if (!nums[i]) continue; 
    min = Math.min(min, nums[i]);
    max = Math.max(max, nums[i]);
    if (!repeated.includes(nums[i])) {
      repeated.push(nums[i]);
    } else {
      return false;
    }
  }
  if (max - min + 1 > 5) return false;
  return true;
};
```