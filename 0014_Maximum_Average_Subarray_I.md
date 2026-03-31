# Maximum Average Subarray I

## 2026.3.31

## description
You are given an integer array `nums` consisting of `n` elements, and an integer `k`.

Find a contiguous subarray whose length is equal to `k` that has the maximum average value and return this value. Any answer with a calculation error less than `10^-5` will be accepted.

Example 1:  
Input: nums = [1,12,-5,-6,50,3], k = 4  
Output: 12.75000  
Explanation: Maximum average is `(12 - 5 - 6 + 50) / 4 = 51 / 4 = 12.75`

Example 2:  
Input: nums = [5], k = 1  
Output: 5.00000

## solution_1
```python
class Solution:
    def findMaxAverage(self, nums: List[int], k: int) -> float:
        current_sum = sum(nums[:k])
        max_sum = current_sum

        for i in range(1, len(nums) - k + 1):
            current_sum += nums[i + k - 1] - nums[i - 1]
            max_sum = max(current_sum, max_sum)

        return max_sum / k
```

## methods
__sliding window__

这题要求的是“长度固定为 `k` 的连续子数组”中的最大平均值。  
因为每个窗口长度都一样，所以比较平均值大小，其实就等价于比较窗口总和大小。

核心思路：

1. 先求出第一个长度为 `k` 的窗口和，记为 `current_sum`
2. 用 `max_sum` 记录目前遇到的最大窗口和
3. 窗口每次向右移动一格：
   - 减去左边移出的数 `nums[i - 1]`
   - 加上右边新进入的数 `nums[i + k - 1]`（关键，不用重复求和）
4. 每移动一次，就更新一次 `max_sum`
5. 最后返回 `max_sum / k`

这样就不需要每次都重新计算一个窗口的总和，效率更高。

## M&L
1. 看到“固定长度的连续子数组”，优先想到滑动窗口。
2. 这题比较平均值时，因为所有窗口长度都是 `k`，所以可以直接比较窗口和，不必每次都真的去算平均值。
3. 初始化时一定要写 `sum(nums[:k])`，不能少写成 `sum(nums[:k - 1])`，否则第一个窗口就不完整（切片左开右闭）。
4. 窗口右移时的更新公式要熟：`current_sum += nums[i + k - 1] - nums[i - 1]`
5. 时间复杂度是 `O(n)`，空间复杂度是 `O(1)`。
6. 变量在被读取之前必须要先被赋值。