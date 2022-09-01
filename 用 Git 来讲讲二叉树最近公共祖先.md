# labuladong-python3

遇到任何递归型的问题，无非就是灵魂三问：

1、这个函数是干嘛的？

2、这个函数参数中的变量是什么的是什么？

3、得到函数的递归结果，你应该干什么？

类似动态规划套路
「定义」「状态」「选择」

### 236 二叉树的最近公共祖先
```
# 因为是要最近公共祖先  深度最大  所以选定使用后续遍历的框架
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
        # base case
        if not root:
            return None
        if root == p or root == q:
            return root
        left = self.lowestCommonAncestor(root.left, p, q)
        right = self.lowestCommonAncestor(root.right, p, q)
        # 情况1
        if left and right:
            return root
        # 情况2
        if not left and not right:
            return None
        # 情况3
        return right if not left else left
```
