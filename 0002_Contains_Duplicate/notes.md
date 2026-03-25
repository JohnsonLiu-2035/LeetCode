# Contains Duplicate

## 2026.3.18

## description
Given an integer array nums, return true if any value appears at least twice in the array, and return false if every element is distinct.

## solution_1
```python
class Solution:
    def containsDuplicate(self, nums: List[int]) -> bool:
        new_nums = set(nums)
        if len(new_nums) != len(nums):
            return True
        else:
            return False
```

## method
利用tuple的去重特性，去掉重复值，比较序列长度。

## mistakes&lessons 
1. tuple可以遍历，可以用len()测量长度。     
2. 内置函数set()是在python底部的C中实现，无for遍历，此方法complexity最低。     


## solution_2
```python
class Solution:
    def containsDuplicate(self, nums: List[int]) -> bool:
        hashmap = {}
        for i,j in enumerate(nums):
            hashmap[j] = i
        if len(hashmap) != len(nums):
            return True
        else:
            return False
```

## method
利用字典键重复时自动掩盖前者的特性，消重比长度。

## mistakes&lessons
1. for循环所有数，键值存入Hashmap，complexity太高。      
2. 应该考虑online做法，避免全部存入再查找。     
3. len(dict)可以返回字典的键的个数。     

## solution_3
```python
class Solution:
    def containsDuplicate(self, nums: List[int]) -> bool:
        hashmap = {}
        for i in nums:
            if i in hashmap:
                return True
            hashmap[i] = 1
        return False   
```

## method
online方法，先查找再添加，灵感来自于onepass hashmap_twosum。能够降低solution_2的复杂度。

## mistakes&lessons
1. online process降低复杂度。   
2. 先查找再添加的遍历技巧。

## solution_4
```python
class Solution:
    def containsDuplicate(self, nums: List[int]) -> bool:
        seen = set()
        for i in nums:
            if i in seen:
                return True
            seen.add(i)
        return False
```

## method
online process的set()版本，先查再存。

## mistakes&lessons
1. set.add()
