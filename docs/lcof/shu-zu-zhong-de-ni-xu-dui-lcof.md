# 面试题51. 数组中的逆序对

在数组中的两个数字，如果前面一个数字大于后面的数字，则这两个数字组成一个逆序对。输入一个数组，求出这个数组中的逆序对的总数。

 

示例 1:

输入: [7,5,6,4]
输出: 5
 

限制：

0 <= 数组长度 <= 50000


来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/shu-zu-zhong-de-ni-xu-dui-lcof

## 1.暴力双迭代（超时）

```js
/**
 * 暴力双迭代，时间复杂度O(n^2)
 * @param {number[]} nums
 * @return {number}
 */
var reversePairs1 = function(nums) {
  if (nums.length < 2) return 0;
  let count = 0;
  for (let i = 0; i < nums.length - 1; i++) {
    for (let j = i + 1; j < nums.length; j++) {
      if (nums[i] > nums[j]) {
        count++;
      }
    }
  }
  return count;
};
```

## 2.归并排序

```js
/**
 * 在使用归并排序时，拆分的过程是不会打乱数组，
 * 使用归并排序成左小右大的时，会先将拆分出的两个数组中小的一个进入临时数组
 * 在添加左数组进入临时数组时，假如在此前右数组已经添加了一部分元素进临时数组，说明这些元素是
 * 比当前要添加临时数组的左数组元素要小，又因为这些元素在右数组中，说明这些元素在原数组中是在当前左元素后面的，
 * 换言之，这些元素可以与当前左数组元素可以构成逆序对，那么会构成多少个呢？
 * 只需要统计一下目前右数组已经添加了多少个元素到临时数组就好！
 * @param {number[]} nums
 * @return {number}
 */
var reversePairs2 = function(nums) {
  if (nums.length < 2) return 0; 
  let count = 0;;
  // [left, right]
  function mergeSort(numArr, left, right) {

    if (right - left === 0) {
      return [numArr[left]];
    }
    const mid = left + Math.floor((right - left) / 2);
    const leftArr = mergeSort(numArr, left, mid);
    const rightArr = mergeSort(numArr, mid + 1, right);
    return merge(leftArr, rightArr);
  }
  function merge(leftArr, rightArr) {
    let tmp = [];
    let lIdx = 0;
    let rIdx = 0;
    while (lIdx < leftArr.length && rIdx < rightArr.length) {
      // 注意这里要加上等号
      // 因为相同的值不算逆序对，没有等号时，就是将right中的元素添加到tmp中，rIdx就会步进，
      // 相当于相同值也算进了逆序对
      if (leftArr[lIdx] <= rightArr[rIdx]) {
        tmp.push(leftArr[lIdx]);
        lIdx++;
        count += rIdx
      } else {
        tmp.push(rightArr[rIdx]);
        rIdx++;
      }
    }

    while (lIdx < leftArr.length) {
      tmp.push(leftArr[lIdx]);
      lIdx++;
      count += rIdx;
    }

    while (rIdx < rightArr.length) {
      tmp.push(rightArr[rIdx]);
      rIdx++;
    }

    return tmp;
  }

  mergeSort(nums, 0, nums.length - 1);
  return count;
}
```