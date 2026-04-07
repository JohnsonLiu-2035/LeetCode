# Valid Parentheses

## 2026.4.7

## description
Given a string `s` containing just the characters `'('`, `')'`, `'{'`, `'}'`, `'['` and `']'`, determine if the input string is valid.

An input string is valid if:

1. Open brackets must be closed by the same type of brackets.
2. Open brackets must be closed in the correct order.
3. Every close bracket has a corresponding open bracket of the same type.

Example 1:  
Input: s = "()"  
Output: true

Example 2:  
Input: s = "()[]{}"  
Output: true

Example 3:  
Input: s = "(]"  
Output: false

## solution_1
```python
class Solution:
    def isValid(self, s: str) -> bool:
        stack = []
        pairs = {
            ')': '(',
            ']': '[',
            '}': '{'
        }

        for ch in s:
            if ch in '([{':
                stack.append(ch)
            else:
                if not stack or stack[-1] != pairs[ch]:
                    return False
                stack.pop()

        return not stack
```

## methods
__stack + hashmap__

这题的核心不是“数括号个数”，而是“维护最近一个还没被匹配的左括号”。  
所以最适合的数据结构就是 `stack` / 栈。

遍历字符串时：
+
1. 如果当前字符是左括号，就先压入栈中，表示它还在等待匹配。
2. 如果当前字符是右括号，就必须拿它去和栈顶的左括号配对。
3. 如果栈为空，说明当前右括号前面根本没有可匹配的左括号，直接 `False`。
4. 如果栈顶类型不匹配，也直接 `False`。
5. 只有匹配成功时，才能把栈顶弹出（`pop`），表示这一对括号已经处理完。
6. 最后如果栈为空，说明所有左括号都被正确匹配；否则还有残留左括号，返回 `False`。

`pairs` 这个 hashmap 的巧思在于：

```python
pairs = {
    ')': '(',
    ']': '[',
    '}': '{'
}
```

它把“右括号应该匹配哪个左括号”直接存起来。  
这样遇到右括号时，不需要写一长串条件判断，只需要比较：

```python
stack[-1] == pairs[ch]
```

逻辑会非常清晰。

## M&L
1. 这题最重要的思想是“后进先出”。最后出现的左括号，必须最先被匹配，所以要想到栈。
2. 我这次最大的错误是写了 `if cha == '(' or '{' or '['`。这个条件在 Python 里会恒为 `True`，因为非空字符串本身就是真值。这里必须写成 `if ch in '([{'`，既简洁又不容易错。
3. `not stack` 在这题里有两次很关键的意义：
   - 遇到右括号时，先判断栈是不是空的，避免访问 `stack[-1]` 报错。
   - 最后用 `return not stack` 判断是否还有没匹配完的左括号。
4. `else` 里面再嵌套一个 `if` ，因为“不是左括号”并不等于“就合法”，而是进入“校验右括号是否匹配”的分支。不能省略这个`else`。
5. `if not stack or stack[-1] != pairs[ch]:` 这个顺序不能反。必须先判断 `not stack`，因为空栈时先访问 `stack[-1]` 会直接报错。
6. `stack.pop()` 的含义不是“删除一个元素”这么简单，而是“当前右括号已经和最近的左括号成功配对，所以把这一对从待处理状态里移除”。
7. 这题让我再次体会到：当数据不是普通数字，而是“成对关系”“匹配关系”“顺序关系”时，先想清楚数据结构比上来写条件判断更重要。这里用 `stack + hashmap`，就比手写很多 `if-elif` 更稳。
8. 以后遇到括号、标签匹配、路径回退这类题，要优先联想到栈；遇到“固定映射关系”，要优先联想到 hashmap。
