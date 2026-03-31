# Remove Duplicates from Sorted Array

## 2026.3.30

## description
Given an integer array `nums` sorted in non-decreasing order, remove the duplicates in-place such that each unique element appears only once.  
The relative order of the elements should be kept the same.  
Then return the number of unique elements in `nums`.  

Consider the number of unique elements of `nums` to be `k`, to get accepted, you need to do the following things:

- Change the array `nums` such that the first `k` elements of `nums` contain the unique elements in the order they were present in `nums` initially.
- The remaining elements of `nums` are not important as well as the size of `nums`.
- Return `k`.

Example 1:  
Input: nums = [1,1,2]  
Output: 2, nums = [1,2,_]  

Example 2:  
Input: nums = [0,0,1,1,1,2,2,3,3,4]  
Output: 5, nums = [0,1,2,3,4,_,_,_,_,_]  

## ***solution_1***
```python
class Solution:
    def removeDuplicates(self, nums: List[int]) -> int:
        if not nums:
            return 0
        left = 1
        for right in range(1, len(nums)):
            if nums[right] != nums[left - 1]:
                nums[left] = nums[right]
                left += 1
        return left
```

## methods
__two pointers双指针__  
因为数组已经排好序，所以重复元素一定会连续出现。  
`right` 用来从左到右扫描整个数组（扫描指针），`left` 指向下一个应该放置不重复元素的位置（填充指针）。  
每次判断 `nums[right]` 是否和当前最后一个不重复元素 `nums[left - 1]` 相同：  
如果不同，说明找到了一个新的不重复元素，就把它放到 `nums[left]`，然后让 `left += 1`。  
遍历结束后，`left` 的值就是不重复元素的个数。  

这个做法只遍历一次数组，时间复杂度是 `O(n)`，额外空间复杂度是 `O(1)`。  

## M&L
1. 这题的关键前提是数组已经排序，因此可以直接通过相邻的不重复段来去重。  
2. `left` 从 `1` 开始，是因为第一个元素一定可以保留；后面只需要决定哪些元素要写到它后面。  
3. 判断条件写成 `nums[right] != nums[left - 1]` 很巧，它表示“当前扫描到的值是否不同于结果区最后一个值”。  
4. 如果 `nums` 为空，这段代码会返回 `1`，所以它依赖题目测试用例至少有一个元素；故先判断 `if not nums: return 0`。  
