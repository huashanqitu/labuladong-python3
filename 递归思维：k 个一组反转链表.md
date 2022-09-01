# labuladong-python3

### 反转链表 迭代法
```
def reverse(a):
    cur = a
    next = a
    pre = None
    while cur:
        next = cur.next
        ## 逐个节点反转
        cur.next = pre
        ## 更新指针位置
        pre = cur
        cur = next
    ## 返回反转后的头结点
    return pre
```

### 反转 a 到 b 之间的结点
```
## 反转区间 [a, b) 的元素，注意是左闭右开
def reverse(a, b):
    pre = None
    cur = a
    next = a
    while cur != b:
        next = cur.next
        cur.next = pre
        pre = cur
        cur = next
    return pre
```    


### 25 K 个一组反转链表

```
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def reverseKGroup(self, head, k):
        if not head:
            return head
        ## 区间 [a, b) 包含 k 个待反转元素
        a = head
        b = head
        for i in range(k):
            ## 不足 k 个，不需要反转，base case
            if not b:
                return a
            b = b.next
        ## 反转前k个元素
        newHead = self.reverse(a,b)
        ## 递归反转后续链表并连接起来
        a.next = self.reverseKGroup(b, k)
        return newHead

    def reverse(self, a, b):
        pre = None
        cur = a
        next = a
        while cur != b:
            next = cur.next
            cur.next = pre
            pre = cur
            cur = next
        return pre
```        
