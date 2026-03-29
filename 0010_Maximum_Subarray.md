# Maximum Subarray

## 2026.3.29

## description
Given an integer array nums, find the subarray with the largest sum, and return its sum.  

Example 1:  
Input: nums = [-2,1,-3,4,-1,2,1,-5,4]  
Output: 6  
Explanation: The subarray [4,-1,2,1] has the largest sum 6.  

Example 2:  
Input: nums = [1]  
Output: 1  

Example 3:  
Input: nums = [5,4,-1,7,8]  
Output: 23  

## solution_1
```python
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        currentsum = maxsum = nums[0]
        for num in nums[1:]:
            currentsum = max(currentsum + num, num)
            maxsum = max(maxsum, currentsum)
        return maxsum
```

## methods
__动态规划（Kadane's Algorithm）__。  
维护两个变量：  
`currentsum` 表示“以当前元素结尾的最大子数组和”，每次比较“把当前数字接到前面的子数组后面”还是“从下一个数字重新开始”。  
`maxsum` 用来记录遍历过程中出现过的全局最大值。  

## M&L
1. 用 `max()/min()` 维护状态很简洁，可以替代较多的 `if/else` 判断。  
2. `currentsum` 不是“到当前位置为止的总和”，而是“必须以当前位置结尾”的最大和。  
3. 初始化要从 `nums[0]` 开始，不能默认写成 `0`，否则全为负数时会出错。  
4. 这种题的关键是先想清楚“状态定义”再写代码，代码会更顺。  

## solution_2
```python
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        maxSum = nums[0] ## 或者 maxsum = float('inf')
        currentSum = 0
        for num in nums:
            currentSum += num
            if currentSum > maxSum:
                maxSum = currentSum
            if currentSum < 0:
                currentSum = 0
        return maxSum
```

## methods
__Kadane's Algorithm__ 的贪心算法写法。  
`currentSum` 表示当前这段连续子数组的和，每次先把当前数字加进去，再更新 `maxSum`。  
复数前缀不保留，如果 `currentSum` 已经小于 `0`，说明它只会拖累后面的结果，所以直接清零，从下一个位置重新开始累计。  

## M&L
1. 这一版要特别注意 `maxSum` 不能初始化为 `0`，否则全负数数组会出错。  
2. 更新 `maxSum` 的操作要放在 `currentSum < 0` 清零之前，不然会漏掉负数里的最大值。  
3. 这版更偏“贪心”理解，核心想法是负数前缀没有保留价值。  
4. 这一版的复杂度更低，但代码不如第一版的美观。  
5. 可以初始赋值`float('-inf')`，让这个变量足够小。  
