# Binary_Search

## 2026.3.26

## description
Given an array of integers `nums` which is sorted in ascending order, and an integer `target`, write a function to search `target` in `nums`.  
If `target` exists, return its index. Otherwise, return `-1`.
You must write an algorithm with `O(log n)` runtime complexity.

## ***solution***
```python
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        left = 0
        right = len(nums) - 1

        while left <= right:
            mid = left + (right - left) // 2

            if nums[mid] == target:
                return mid
            elif nums[mid] < target:
                left = mid + 1
            else:
                right = mid - 1

        return -1
```

## method
__二分查找__。  
使用闭区间 `[left, right]`，每轮比较 `nums[mid]` 和 `target`：  
如果相等，直接返回 `mid`；  
如果 `nums[mid] < target`，说明目标只可能在右半区，更新 `left = mid + 1`；  
如果 `nums[mid] > target`，说明目标只可能在左半区，更新 `right = mid - 1`。  
当 `left > right` 时循环结束，表示数组中不存在目标值，返回 `-1`。

## M&L
1. 闭区间写法要配套 `while left <= right`，否则可能漏掉边界元素。  
2. 边界收缩必须写成 `mid + 1` 或 `mid - 1`，避免死循环。  
3. `mid = left + (right - left) // 2` 是更稳妥的写法，跨语言时可避免整型溢出风险。  
4. 二分查找的本质是每轮排除一半区间，因此时间复杂度是 `O(log n)`。
