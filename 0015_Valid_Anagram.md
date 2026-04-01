# Valid Anagram

## 2026.4.1

## description
Given two strings `s` and `t`, return `true` if `t` is an anagram of `s`, and `false` otherwise.

An Anagram is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.

Example 1:  
Input: s = "anagram", t = "nagaram"  
Output: true

Example 2:  
Input: s = "rat", t = "car"  
Output: false

## ***solution_1***
```python
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        if len(s) != len(t):
            return False
        count = {}
        for i in s:
            count[i] = count.get(i , 0) + 1
        for j in t:
            count[j] = count.get(j , 0) - 1
            if count[j] < 0:
                return False
        return True
```

## methods
__hashmap__

这题的核心是判断两个字符串中每个字符出现的次数是否完全一致。  
如果长度不同，直接返回 `False`，因为字符总数已经对不上。  

具体做法：
1. 先遍历字符串 `s`，用字典 `count` 统计每个字符出现的次数  
2. 再遍历字符串 `t`，每遇到一个字符，就在字典里把对应次数减 `1`
3. 如果某个字符减完之后小于 `0`，说明 `t` 中这个字符出现次数更多，直接返回 `False`
4. 如果整个遍历过程都没有问题，说明两个字符串字符频次一致，返回 `True`

这个方法本质上是用哈希表做字符频次统计。  
时间复杂度是 `O(n)`，空间复杂度是 `O(1)`（如果只考虑固定字符集，也可以写成 `O(k)`）。

## M&L
1. 判断是否是 anagram，不是看字符顺序，而是看每个字符的出现次数。  
2. 先判断 `len(s) != len(t)` 可以提前排除很多情况。  
3. `count.get(x, 0)` 很适合做频次统计，避免 key 不存在时报错。  
4. 第二轮遍历时一旦出现 `count[j] < 0`，就可以立刻返回，不需要继续遍历。  
5. 这种“加一遍、减一遍”的写法，是 hashmap 题里很常见的思路。  

## solution_2
```python
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        return sorted(s) == sorted(t)
```

## methods
先把两个字符串分别排序。  
如果排序后的结果完全相同，说明它们包含的字符和每个字符的个数都一样，所以是 anagram。  

这个方法写法最短、最好理解，但排序的时间复杂度是 `O(n log n)`，比 hashmap 解法慢一些。

## M&L
1. 排序法很直观，适合先快速写出可用解。  
2. 只要排序后两个字符串相同，就说明字符组成完全一致。  
3. 这题更优解还是 hashmap，因为它可以做到线性时间复杂度。  

## solution_3
```python
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        s_list = list(s)

        for i in t:
            if i in s_list:
                s_list.remove(i)
            else:
                return False

        if len(s_list) > 0:
            return False

        return True
```

## methods
__list remove__

先把 `s` 转成列表，遍历 `t`。  
如果 `t` 里的字符能在列表里找到，就删除一个；如果找不到，返回 `False`。  
最后如果列表被刚好删空，说明两个字符串匹配成功。

这个方法思路也比较直接，但 `in` 查找和 `remove()` 删除都可能是 `O(n)`，所以整体复杂度容易变成 `O(n^2)`，效率最低。

## M&L
1. 这个方法能做出来，但不适合作为主解法。  
2. 列表删除元素会带来额外移动开销，效率不高。  
3. 遇到“统计出现次数”的题，优先考虑 hashmap，而不是反复查找和删除。  
