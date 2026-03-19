# Majority Element

## 2026.3.19

## description
Given an array nums of size n, return the majority element, which has appeared more than 2/n times.

## solution_1
```python
class Solution:
    def majorityElement(self, nums: List[int]) -> int:
        hashmap = {}
        for i in nums:
            hashmap[i] = hashmap.get(i,0) + 1
            if hashmap[i] > len(nums) / 2:
                return i
```

## method
用字典统计每个元素出现的次数。   
每遍历到一个数，就把它在字典里的次数加一。   
加完以后立刻判断它的次数有没有超过数组长度的一半；如果超过，说明它就是 majority element，直接返回。   

## mistakes&lessons
1.time and space comlexity are too high.   
2.hashmap.get(i,0) + 1：字典循环计数方法。   



