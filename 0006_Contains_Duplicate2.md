# Contains_Duplicate2

## 2026.3.24

## description
Given an integer array `nums` and an integer `k`, return `true` if there are two distinct indices `i` and `j` in the array such that `nums[i] == nums[j]` and `abs(i - j) <= k`

## solution_1(original thought but not the best)
```python
class Solution:
    def containsNearbyDuplicate(self, nums: List[int], k: int) -> bool:
        if len(nums) <= 1:
            return False
        if len(nums) <= k + 1:
            if len(set(nums)) < len(nums):
                return True
            return False
        else:
            window = set(nums[0 : k + 1])
            if len(window) < k + 1:
                return True
            for i in range(1,len(nums) - k):
                window.remove(nums[i - 1])
                if nums[i + k] in window:
                    return True
                window.add(nums[i + k])
            return False
```

## method
生成一个k+1长度的右滑整体窗口，加条件分类讨论判断TF。

## M&L
1. 代码太繁琐，需要单独加很多限制条件。
2. 静态思维方法，不够灵活适配。

## ***solution_2***
```python
class Solution:
    def containsNearbyDuplicate(self, nums: List[int], k: int) -> bool:
        hashmap = {}
        for i,num in enumerate(nums):
            if num in hashmap and abs(hashmap[num] - i) <= k:
                return True
            hashmap[num] = i
        return False
```

## method
enumerate查找键值     
__哈希表__ 存储{num:index}     
如果哈希表中有该遍历的键，且二者index差值小于k，则True    
否则继续遍历，直至完成返回False     

## M&L
1. 哈希表能够查找/储存一个值对应的信息。  
2. enumerate()返回(index,value)的迭代器。  
3. 先查再存模式（先判断再添加）。    

## ***solution_3***
```python
class Solution:
    def containsNearbyDuplicate(self, nums: List[int], k: int) -> bool:
        window = set()
        for i,num in enumerate(nums):
            if num in window:
                return True
            window.add(num)
            if len(window)> k:
                window.remove(nums[i - k])
        return False
```

## method
滑动窗口 "sliding window"算法，维护一个长度逐渐增加到k的窗口，增加末端，删除头端。

## M&L
1. 用动态的思维替代静态的思维。  
2. a = {}是创建一个空字典（哈希表），a = set()是创建空集合。