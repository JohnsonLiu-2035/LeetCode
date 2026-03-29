# Search_Insert_Position

## 2026.3.25

## description
Given a sorted array of distinct integers `nums` and a target value `target`, return the index if the target is found. If not, return the index where it would be if it were inserted in order.

You must write an algorithm with `O(log n)` runtime complexity.

## solution_1
```python
class Solution:
    def searchInsert(self, nums: List[int], target: int) -> int:
        if target in nums:
            return nums.index(target)
        else:
            for i in nums:
                if i > target:
                    return nums.index(i)
            return nums.index(i) + 1
```

## method
先判断 `target` 是否已经存在于 `nums` 中。  
如果存在，就直接返回它的下标。  
如果不存在，就从左到右遍历数组，找到第一个大于 `target` 的元素。  
这个元素所在的位置，就是 `target` 应该插入的位置。  
如果遍历结束都没有找到比 `target` 更大的数，说明 `target` 应该插入到数组末尾。  

## M&L
1. 这个解法比较直观，适合先把题意翻译成代码。  
2. `in` 和 `index()` 虽然写起来方便，但本质上都需要遍历数组，所以效率不高。  
3. 题目要求时间复杂度为 `O(log n)`，后续更优解法应该考虑二分查找。  

## ***solution_2***
```python
class Solution:
    def searchInsert(self, nums: List[int], target: int) -> int:
        left = 0
        right = len(nums) - 1
        while left <= right:
            mid = (right + left) // 2
            if nums[mid] == target:
                return mid
            elif nums[mid] > target:
                right = mid - 1
            else:
                left = mid + 1
        return left
```

## method
__二分查找 Binary Search__，因为数组本身已经是升序排列。  
先定义左右边界 `left` 和 `right`，每次取中间位置 `mid` 和 `target` 进行比较。  
如果 `nums[mid] == target`，说明已经找到目标值，直接返回 `mid`。  
如果 `nums[mid] > target`，说明目标值只可能出现在左半边，所以把右边界更新为 `mid - 1`。  
如果 `nums[mid] < target`，说明目标值只可能出现在右半边，所以把左边界更新为 `mid + 1`。  
当循环结束后，`left` 停留的位置就是 `target` 应该插入的位置，因此返回 `left` 即可。  

## M&L
1. 看到有序数组并且题目要求 `O(log n)`，要优先想到二分查找。  
2. 二分查找不只是用来找目标值，也可以用来确定目标值应该插入的位置(区分lower/upper bound)。  
3. 这题最关键的地方是循环结束后返回 `left`，因为它正好表示插入位置（二分查找用mid + 1和mid - 1）。  
4. 整除 // 向下取整。  

