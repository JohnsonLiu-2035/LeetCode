# Squares of a Sorted Array

## 2026.3.31

## description
Given an integer array `nums` sorted in non-decreasing order, return an array of the squares of each number sorted in non-decreasing order.

Example 1:  
Input: nums = [-4,-1,0,3,10]  
Output: [0,1,9,16,100]  

Example 2:  
Input: nums = [-7,-3,2,3,11]  
Output: [4,9,9,49,121]  

## ***solution_1***
```python
class Solution:
    def sortedSquares(self, nums: List[int]) -> List[int]:
        right_pos = len(nums) - 1
        res = [0] * len(nums)
        left_pointer = 0
        right_pointer = len(nums) - 1

        while left_pointer <= right_pointer:
            left_square = nums[left_pointer] ** 2
            right_square = nums[right_pointer] ** 2

            if left_square < right_square:
                res[right_pos] = right_square
                right_pointer -= 1
            else:
                res[right_pos] = left_square
                left_pointer += 1

            right_pos -= 1

        return res
```

## methods
__two pointers__

这题的关键在于：原数组虽然是递增的，但平方之后，最大值只可能出现在最左边或最右边。  
因为负数平方后可能很大，所以不能直接从左到右平方后认为结果还是有序的。  

用两个指针分别指向数组两端：  

- `left_pointer` 指向最左边  
- `right_pointer` 指向最右边  
- `right_pos` 指向结果数组当前要填入的位置，从后往前放  
- `res` 创建一个和nums长度相等的全o序列  

每次比较 `nums[left_pointer] ** 2` 和 `nums[right_pointer] ** 2`：

- 如果右边更大，就把右边平方放进 `res[right_pos]`，然后 `right_pointer -= 1`
- 否则把左边平方放进 `res[right_pos]`，然后 `left_pointer += 1`

这样每一轮都把“当前最大平方值”放到结果数组的末尾，最终得到递增结果。

时间复杂度是 `O(n)`，空间复杂度是 `O(n)`。

## M&L
1. 看到“有序数组 + 平方后重新排序”，要先想到平方会破坏原有顺序，尤其是负数部分。
2. 这题不是在原数组里直接找递增关系，而是在两端比较绝对值大小。
3. 结果数组从后往前填，是因为每次拿到的都是当前最大的平方值。
4. 你原来贴的双指针代码里，`left_pointer += 1` 不能写在 `if/else` 外面；只有左边平方被选中时，左指针才应该右移。

## solution_2
```python
class Solution:
    def sortedSquares(self, nums: List[int]) -> List[int]:
        for i in range(len(nums)):
            nums[i] = nums[i] ** 2
        nums.sort()
        return nums
```

## methods
__square first, sort later__

这版思路最直接：

1. 先把每个元素原地平方
2. 再对整个数组排序
3. 返回排序后的结果

优点是好写、好理解。  
缺点是排序这一步需要 `O(n log n)`，不如双指针的线性做法高效。

## M&L
1. 如果面试或刷题时先想到这版，也可以先写出来，至少保证正确性。
2. 这版利用了 Python 内置排序`sort()`，代码短，但没有用到“原数组已经有序”这个条件。
3. 当题目本身给了“sorted in non-decreasing order”时，通常意味着可以进一步优化到双指针。
