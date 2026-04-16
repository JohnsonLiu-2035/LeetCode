# Climbing Stairs

## 2026.4.16

## description
You are climbing a staircase. It takes `n` steps to reach the top.

Each time you can either climb `1` or `2` steps. In how many distinct ways can you climb to the top?

Example 1:  
Input: n = 2  
Output: 2

Example 2:  
Input: n = 3  
Output: 3

## solution_1
```python
class Solution:
    def climbStairs(self, n: int) -> int:
        count = 0
        left = 0
        right = 1
        while count < n:
            cur = left + right
            left = right
            right = cur
            count += 1
        return cur
```

## methods
__Fibonacci + rolling variables__

这版是把题目当成斐波那契数列来推。

因为每次只能走 `1` 步或者 `2` 步，所以走到第 `n` 阶的方法数，一定等于：

```python
f(n) = f(n - 1) + f(n - 2)
```

这和斐波那契的递推关系完全一样。

这版写法是从更原始的斐波那契起点 `0, 1` 开始滚动：

- `left` 保存前一个数
- `right` 保存当前数
- `cur` 保存新算出来的数

每轮都做：

```python
cur = left + right
left = right
right = cur
```

`while + count` 的作用，本质上就是手动控制一共往前推多少轮。

例如：

- 第 1 轮得到 `1`
- 第 2 轮得到 `2`
- 第 3 轮得到 `3`
- 第 4 轮得到 `5`

刚好和这题的答案序列对应。

时间复杂度是 `O(n)`，空间复杂度是 `O(1)`。

## M&L
1. 这版的核心不是 DP 数组，而是发现它满足斐波那契式递推。
2. `while count < n` 本质上是在模拟“做 `n` 次状态推进”。
3. 这版是从 `0, 1` 起步，所以更像数学上的斐波那契写法。
4. 这题在 LeetCode 里通常 `n >= 1`，所以这版能直接用；如果考虑 `n = 0`，就需要额外处理，否则 `cur` 可能没有定义。

## ***solution_2***
```python
class Solution:
    def climbStairs(self, n: int) -> int:
        if n <= 2:
            return n 
        first = 1
        second = 2
        for _ in range(3, n + 1):
            first , second = second , first + second
        return second
```

## methods
__dynamic programming(DP) / Fibonacci + rolling variables__

这版更贴近题目本身。

定义：

- `f(1) = 1`
- `f(2) = 2`

然后递推：

```python
f(n) = f(n - 1) + f(n - 2)
```

所以这里没有从斐波那契的 `0, 1` 开始，而是直接从这题自己的边界 `1, 2` 开始。

- `first` 表示前前一个状态
- `second` 表示前一个状态

每一轮更新：

```python
first, second = second, first + second
```

这里的 `for _ in range(3, n + 1)` 可以理解成“假遍历”。
它不是在遍历数组，而是在控制状态从第 `3` 项一路推到第 `n` 项。

和第一版相比，这版有两个优点：

- 不需要额外的 `count`
- 初始值 `1, 2` 直接对应本题，更容易理解

时间复杂度是 `O(n)`，空间复杂度是 `O(1)`。

## M&L
1. 这题本质上就是一个斐波那契型 DP：最后一步只可能从 `n - 1` 或 `n - 2` 过来。
2. `for _ in range(3, n + 1)` 不是遍历数据，而是控制递推轮数，这样可以替代 `while + count`。
3. `first, second = second, first + second` 是最常见的滚动更新写法，能同时完成状态右移和新值计算。
4. 这版直接从 `1, 2` 起步，是因为它们正好就是这题的边界条件 `f(1)` 和 `f(2)`。
5. 如果题目已经能确定只依赖前两个状态，就没必要开数组，两个变量就够了。
