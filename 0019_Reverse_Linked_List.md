# Reverse Linked List

## 2026.4.16

## description
Given the `head` of a singly linked list, reverse the list, and return the reversed list.

Example 1:  
Input: head = [1,2,3,4,5]  
Output: [5,4,3,2,1]

Example 2:  
Input: head = [1,2]  
Output: [2,1]

Example 3:  
Input: head = []  
Output: []

## solution_1
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        prev = None
        cur = head
        while cur:
            nxt = cur.next
            cur.next = prev
            prev = cur
            cur = nxt
        return prev
```

## methods
__linked list + iteration + three pointers__

这题的核心不是新建链表，而是把原来每个节点的 `next` 指向反过来。

一共抓住 3 个指针：

- `cur`：当前正在处理的节点
- `prev`：当前节点反转后应该指向的前一个节点
- `nxt`：提前保存原链表里的下一个节点，防止断链

循环里每次都做同样 4 件事：

1. 先保存 `cur.next` 到 `nxt`
2. 把 `cur.next` 改成指向 `prev`
3. 把 `prev` 往前移到 `cur`
4. 把 `cur` 往前移到 `nxt`

也就是：

```python
nxt = cur.next
cur.next = prev
prev = cur
cur = nxt
```

为什么一定要先存 `nxt`？

因为一旦执行了：

```python
cur.next = prev
```

原来的后继节点就找不到了。先把它存下来，后面才能继续往后走。

当 `cur` 变成 `None` 时，说明原链表已经走完了，而 `prev` 正好停在反转后链表的新头节点，所以最后返回 `prev`。

时间复杂度是 `O(n)`，空间复杂度是 `O(1)`。

## M&L
1. 这题最重要的不是背代码，而是理解 3 个指针各自的职责：`prev` 负责接回去，`cur` 负责当前处理，`nxt` 负责保住后路。
2. `head` 一开始只是原链表的入口。反转完成后，新链表的头节点已经不是 `head`，而是 `prev`，所以不能 `return head`。
3. `cur.next = prev` 改的是指针方向，不是节点值。这题从头到尾都没有改 `val`，只是在重连节点。
4. `prev = None` 很关键，因为反转后原头节点会变成新尾节点，而新尾节点的 `next` 本来就应该是 `None`。
5. 空链表和单节点链表都不用特判。`while cur` 自然就能处理：空链表直接返回 `None`，单节点会正常返回自己。
6. 看到“反转链表”时，要优先想到“断开当前节点，再把它接到前面去”，而不是新建数组或新建一条链表。
