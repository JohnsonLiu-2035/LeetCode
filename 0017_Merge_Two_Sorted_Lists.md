# Merge Two Sorted Lists

## 2026.4.9

## description
You are given the heads of two sorted linked lists `list1` and `list2`.

Merge the two lists into one sorted list. The list should be made by splicing together the nodes of the first two lists.

Return the head of the merged linked list.

Example 1:  
Input: list1 = [1,2,4], list2 = [1,3,4]  
Output: [1,1,2,3,4,4]

Example 2:  
Input: list1 = [], list2 = []  
Output: []

Example 3:  
Input: list1 = [], list2 = [0]  
Output: [0]

## solution_1
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def mergeTwoLists(self, list1: Optional[ListNode], list2: Optional[ListNode]) -> Optional[ListNode]:
        dummy = ListNode()
        cur = dummy

        while list1 and list2:
            if list1.val <= list2.val:
                cur.next = list1
                list1 = list1.next
            else:
                cur.next = list2
                list2 = list2.next
            cur = cur.next

        cur.next = list1 if list1 else list2
        return dummy.next
```

## methods
__linked list + iteration + dummy node__

这题的核心不是新建很多节点，而是把原来两条链表里的节点重新接起来。

需要抓住 3 个指针：

- `list1`：第一条链表当前走到的节点
- `list2`：第二条链表当前走到的节点
- `cur`：结果链表当前的尾节点

`dummy` 是一个虚拟头节点，它只是为了让操作更统一。真正的答案不是 `dummy`，而是 `dummy.next`（`cur`的头节点）。

循环里每次做的事情都一样：

1. 比较 `list1.val` 和 `list2.val`
2. 谁更小，就把谁这个节点接到 `cur.next`
3. 被接走的那条链表往后走一步
4. `cur` 也往后走一步

也就是：

- `cur.next = list1` 不是接值，而是接节点
- `list1 = list1.next` 是让 `list1` 这个指针往后移动
- `cur = cur.next` 是让结果链表的尾指针往后移动

当 `while list1 and list2` 结束时，说明至少有一条链表已经空了。另一条链表剩下的部分有序且一定大于尾指针，所以可以直接整体接到后面：

```python
cur.next = list1 if list1 else list2
```

时间复杂度是 `O(m + n)`，空间复杂度是 `O(1)`。

## M&L
1. 链表里“返回一个节点”，本质上就是“返回从这个节点开始的整段链表”。所以 `return dummy.next` 返回的是合并后真正的头节点，也等于返回整条结果链表。
2. `cur.next` 存的是“下一个节点”，不是值。所以这里必须写 `cur.next = list1`，不能写 `cur.next = list1.val`。
3. `if list1.val <= list2.val` 比较的是两条链表“当前节点”的值。一开始比较的是两个头节点，之后谁被选中，谁就往后走一步。
4. `dummy` 可以理解成辅助用的假表头。它的 `val` 通常无所谓，写 `ListNode()` 和 `ListNode(0)` 在这题里都可以，因为最后返回的是 `dummy.next`。
5. 不能直接 `return dummy`，因为那样会把虚拟头节点一起带进答案。也不能 `return dummy.next.next`，因为那会把第一个真实节点跳过去。
6. 链表题里经常说“head 就是链表”，更准确地说，是“head 是进入整条链表的入口”。只要拿到头节点，就能顺着 `next` 访问后面所有节点。
7. 这题即使一开始有空链表也没问题。因为 `while list1 and list2` 不会进入循环，最后会直接把非空的那条链表接上；如果两条都空，最后返回的就是 `None`。
8. `dummy` 常见的原因是它能帮你统一处理头节点。只要你发现自己在纠结“第一个节点是谁”或者“头节点要不要特判”，通常就该考虑 `dummy`。
