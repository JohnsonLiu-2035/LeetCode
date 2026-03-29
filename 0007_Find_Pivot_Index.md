# Find_Pivot_Index

## 2026.3.25

## description
Given an array of integers `nums`, calculate the pivot index of this array.  
The pivot index is the index where the sum of all the numbers strictly to the left of the index is equal to the sum of all the numbers strictly to the right of the index.  
If no such index exists, return `-1`. If there are multiple pivot indexes, return the left-most pivot index.

## solution
```python
class Solution:
    def pivotIndex(self, nums: List[int]) -> int:
        for i, num in enumerate(nums):
            if sum(nums[:i]) == sum(nums[i + 1 : len(nums)]):
                return i
        return -1
```

## method
暴力遍历每个位置 `i`，分别计算左边和右边区间和。  
左边是 `sum(nums[:i])`，右边是 `sum(nums[i+1:])`。  
如果两边相等，当前 `i` 就是答案，直接返回；遍历完没有命中则返回 `-1`。

## mistakes&lessons
1. 这个写法直观但有重复计算，时间复杂度偏高。  
2. 看到“左右和相等”时，可以优先考虑“总和 + 前缀和”优化。  
3. 题目要求最左下标，找到后应立即返回。

## ***solution_2***
```python
class Solution:
    def pivotIndex(self, nums: List[int]) -> int:
        # 核心：用 left_sum 递推，并由 total_sum - left_sum - num 得到 right_sum
        left_sum = 0
        total_sum = sum(nums)
        for i, num in enumerate(nums):
            right_sum = total_sum - left_sum - num
            if right_sum == left_sum:
                return i
            left_sum += num
        return -1
```

## method
先计算数组总和 `total_sum`。遍历时维护左侧累加和 `left_sum`，当前位置右侧和可以通过  
`right_sum = total_sum - left_sum - num` 直接得到。  
每次只做 O(1) 运算，若 `left_sum == right_sum` 就返回该下标。

## M&L
1. 把“每轮重复求和”改成“预处理一次 + 递推更新”，效率提升明显。  
2. 遇到区间和比较问题，优先想能否用代数关系消掉重复遍历。  
3. `enumerate` 同时拿到下标和值，代码更简洁。  
