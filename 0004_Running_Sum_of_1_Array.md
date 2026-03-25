# Running Sum of one Array

## 2026.3.22

## description
Given an array `nums`. We define a running sum of an array as `runningSum[i] = sum(nums[0]…nums[i])`.     
Return the running sum of `nums`.   

## solution_1
```python
class Solution:
    def runningSum(self, nums: List[int]) -> List[int]:
        runningsum = []
        for i in range(len(nums)):
            if i == 0:
                runningsum.append(nums[i])
            else:
                runningsum.append(nums[i] + runningsum[i-1])
        return runningsum
```

## method
创建新列表，循环新列表前项加旧列表后项，结果放入新列表。

## mistakes&lessons
1. 不够简洁。   
2. 复杂度太高。  

## *solution_2*
```python
class Solution:
    def runningSum(self, nums: List[int]) -> List[int]:
        for i in range(len(nums)):
            if i == 0:
                pass
            else:
                nums[i] = nums[i] + nums[i-1]
        return nums
```
## *solution3*
```python
class Solution:
    def runningSum(self, nums: List[int]) -> List[int]:
        for i in range(1,len(nums)):
            nums[i] = nums[i] + nums[i-1]
        return nums
```

## method
原地更新列表，前项＋后项。

## mistake&lessons
1. 复杂度最低。   
2. pass跳过判断语句执行部分。   
3. 直接从第二项开始，可不用判断语句。   
