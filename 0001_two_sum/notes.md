# Two Sum

## 2026.3.15

## description
Given an array of integers nums and an integer target, return indices of the two numbers such that they add up to target.  
You may assume that each input would have exactly one solution, and you may not use the same element twice.  You can return the answer in any order.    
Example:  
Input: nums = [2,7,11,15], target = 9  
Output: [0,1]  
Explanation: Because nums[0] + nums[1] == 9, we return [0, 1]  

## solution
```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        for i in range(0,len(nums)):
            for j in range(i+1,len(nums)):
                if nums[i] + nums[j] == target:
                    return [i,j]
```

## methods 
用双层嵌套for循环分别遍历两个数。

## mistakes&lessons
1.时间复杂度太高。    
2.LeetCode会自动传参和调用函数，不用手动操作。  
3.注意变量名单复数，避免写错。   
4.报错的argument是实参的意思，parameter是形参。    
5.indices（索引）是index的复数形式。   

