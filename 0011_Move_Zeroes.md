# Move Zeroes

## 2026.3.29

## description
Given an integer array nums, move all `0`'s to the end of it while maintaining the relative order of the non-zero elements.  
Note that you must do this in-place without making a copy of the array.  

Example 1:  
Input: nums = [0,1,0,3,12]  
Output: [1,3,12,0,0]  

Example 2:  
Input: nums = [0]  
Output: [0]  

## solution_1
```python
class Solution:
    def moveZeroes(self, nums: List[int]) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """
        i = 0
        n = len(nums)
        while i < n:
            if nums[i] == 0:
                nums.append(nums[i])
                nums.pop(i)
                n -= 1
            else:
                i += 1
```

## methods
模拟数组移动。  
从前往后遍历数组，遇到 `0` 就把它追加到数组末尾，再把当前位置的 `0` 删除。  
因为删除后后面的元素会整体左移，所以此时 `i` 不能增加，要继续检查当前位置的新元素。  

## M&L
1. 思路直观，容易想到。  
2. `pop(i)` 会导致后面的元素整体前移，所以遇到 `0` 时不能立刻 `i += 1`。  
3. 需要额外维护 `n`，因为每次把 `0` 移到末尾后，后面的新增部分不需要再重复检查。  
4. 这版时间复杂度不是最优，因为 `pop(i)` 在中间删除元素会产生位移，整体更接近 `O(n^2)`。  

## solution_2
```python
class Solution:
    def moveZeroes(self, nums: List[int]) -> None:
        left = 0
        for right in range(len(nums)):
            if nums[right] != 0:
                nums[left], nums[right] = nums[right], nums[left]
                left += 1
```

## methods
__双指针（two pointers）__。  
从左到右扫描，把非0数按原顺序放到最前面。  
`right` 负责遍历整个数组，`left` 负责指向下一个应该放非零元素的位置。 
因为 `right` 先行，所以left除了和right重合时，始终是0元素的索引。  
当 `nums[right] ` 为 `0` 时，跳过该元素。  
当 `nums[right]` 不是 `0` 时，就把它交换到 `left` 的位置，然后让 `left` 往后移动一格。  
这样遍历结束后，前面都是按原相对顺序排好的非零元素，后面自然就是 `0`。  

## M&L
1. 这版的核心不是“看见 `0` 怎么处理”，而是“看见非零数就往前放”。  
2. `left` 左边始终表示已经整理好的非零区间，这是这题最重要的指针含义。  
3. 当 `left == right` 时，交换自己和自己也没问题，不影响结果。  
4. 这版时间复杂度是 `O(n)`，空间复杂度是 `O(1)`，比第一版更优。  
