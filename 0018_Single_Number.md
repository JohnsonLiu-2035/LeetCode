# Single Number

## 2026.4.14

## description
Given a non-empty array of integers `nums`, every element appears twice except for one. Find that single one.

You must implement a solution with a linear runtime complexity and use only constant extra space.

Example 1:  
Input: nums = [2,2,1]  
Output: 1

Example 2:  
Input: nums = [4,1,2,1,2]  
Output: 4

Example 3:  
Input: nums = [1]  
Output: 1

## solution_1
```python
class Solution:
    def singleNumber(self, nums: List[int]) -> int:
        origin = 0
        for num in nums:
            origin ^= num
        return origin
```

## methods
__位运算 + 异或(XOR)__

这题的关键是异或 `^` 的两个性质：

1. `a ^ a = 0`
2. `a ^ 0 = a`

所以数组里成对出现的数字，异或之后都会互相抵消，最后剩下的就是只出现一次的那个数字。

更底层一点理解，异或可以按位来看。  
对于每一个二进制位，结果只取决于这一位上出现了多少个 `1`：

- 如果 `1` 的个数是偶数，结果就是 `0`
- 如果 `1` 的个数是奇数，结果就是 `1`

所以异或的本质，就是按位统计 `1` 的奇偶性。  
而奇偶性和顺序无关，这就是为什么代码里可以一个一个异或：

```python
origin = 0
for num in nums:
    origin ^= num
```

但在分析时，仍然可以把它理解成“相同数字成对抵消”。

例如：

```python
4 ^ 1 ^ 2 ^ 1 ^ 2
= 4 ^ (1 ^ 1) ^ (2 ^ 2)
= 4 ^ 0 ^ 0
= 4
```

时间复杂度是 `O(n)`，空间复杂度是 `O(1)`。

## M&L
1. 这题最重要的不是记公式，而是理解：`XOR` 本质上是在按位统计奇偶性。  
2. 因为奇偶性和顺序无关，所以代码虽然是逐个计算，分析时仍然可以使用交换律和结合律。  
3. `origin` 初始设为 `0` 很重要，因为 `a ^ 0 = a`，不会影响结果。  
4. 看到“其他元素都出现两次，只有一个出现一次”时，要优先联想到异或。  
5. 这题比 `hashmap` 更优，因为同时满足 `O(n)` 时间和 `O(1)` 额外空间。
