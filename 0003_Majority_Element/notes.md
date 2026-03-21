# Majority Element

## 2026.3.19

## description
Given an array `nums` of size n, return the majority element, which has appeared more than n/2 times.

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

## solution_2
```python
from collections import Counter
class Solution:
    def majorityElement(self, nums: List[int]) -> int:
        cnt = Counter(nums)
        for i in cnt:
            if cnt[i] > len(nums)/2:
                return i
```

## solution_3
```python
from collections import Counter
class Solution:
    def majorityElement(self, nums: List[int]) -> int:
        cnt = Counter(nums)
        if cnt.most_common(1)[0][1] > len(nums)/2:
            return cnt.most_common(1)[0][0]
```
## method
从标准库的collections模块中引入Counter类。(solution_2)   
在Counter类中引入.most_common方法，不用遍历。(solution_3)      

## mistakes&lessons  
1.Counter(literal)返回*字典*，可用来统计元素出现个数，降序排列。   
2..most_common(n)统计前n个最多出现的数据，返回[( , ),( , )]，降序排列。  

## *solution_4*
```python
class Solution:
    def majorityElement(self, nums: List[int]) -> int:
        count = candidate = 0
        for i in nums:
            if count == 0:
                candidate = i
            if i == candidate:
                count += 1
            else:
                count -= 1
        return candidate
```
## method
Boyer-Moore Voting Algorithm(摩尔投票算法)      
`candidate` 记录当前候选人，`count` 记录当前候选人的剩余票数。    
遇到相同数字就加一，遇到不同数字就减一，表示不同数字之间互相抵消。    
当 `count` 变成 0 时，说明前面的数字已经抵消完，需要把当前数字设为新的候选人。    
因为 majority element 的数量超过一半，所以最后留下来的候选人一定是它。   

## mistakes&lessons
1.摩尔投票求major element。

## *solution_5*
```python
class Solution:
    def majorityElement(self, nums: List[int]) -> int:
        new_nums = sorted(nums)
        n = len(new_nums)
        return new_nums[n//2]                
```
## method
排序算法   
先用排序把相同的数字放到一起。     
因为 majority element 出现次数大于 `n / 2`，所以它在排序后一定会占据中间位置。     
所以只需要返回排序后数组中间的元素，不需要再额外统计次数。

## mistakes&lessons
1.没有条件判断语句，complexity最低。  
2.巧用排序，使列表整齐化，提高处理可行性。




