# labuladong-python3

### BFS核心框架

队列q就不说了，BFS 的核心数据结构；

cur.adj()泛指cur相邻的节点，比如说二维数组中，cur上下左右四面的位置就是相邻节点；

visited的主要作用是防止走回头路，大部分时候都是必须的，但是像一般的二叉树结构，没有子节点到父节点的指针，不会走回头路就不需要visited
```
from queue import Queue
# 计算从起点start到终点target的最近距离
def BFS(start, target):
    # 核心数据结构
    q = Queue
    # 避免走回头路
    visited = set()

    # 将起点加入队列
    q.put(start)
    visited.add(start)
    # 记录扩散的步数
    step = 0

    while q.qsize() > 0:
        sz = q.qsize()
        # 将当前队列中的所有节点向四周扩散
        for i in range(sz):
            cur = q.get()
            # 这里判断是否到达终点
            if cur is target:
                return step
            # 将cur的相邻节点加入队列 cur.adj()是代表相邻节点
            for x in cur.adj():
                if x not in visited:
                    q.put(x)
                    visited.add(x)
            # 更新步数
            step += 1
```

### 111. 二叉树的最小深度
```
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
from queue import Queue
class Solution:
    def minDepth(self, root: TreeNode) -> int:
        if not root:
            return 0
        q = Queue()
        q.put(root)
        # root 本身就是一层 depth初始化为1
        depth = 1

        while q.qsize() > 0:
            sz = q.qsize()
            # 将当前队列中的所有节点向四周扩散
            for i in range(sz):
                cur = q.get()
                # 判断是否到达终点
                if not cur.left and not cur.right:
                    return depth
                # 将cur的相邻节点加入队列
                if cur.left:
                    q.put(cur.left)
                if cur.right:
                    q.put(cur.right)
            # 增加步数
            depth += 1
        return depth
```

### 752. 打开转盘锁
```
from queue import Queue

class Solution:
    def openLock(self, deadends: List[str], target: str) -> int:
        # 记录已经穷举过的密码 防止走回头路
        visited = set()
        q = Queue()
        # 从起点开始启动广度优先搜索
        step = 0
        q.put("0000")
        visited.add("0000")

        while q.qsize() > 0:
            sz = q.qsize()
            # 将当前队列中的所有节点向周围扩散
            for i in range(sz):
                cur = q.get()
                # 判断是否到达终点
                if cur in deadends:
                    continue
                if cur == target:
                    return step

                # 将一个节点的相邻节点加入队列 不要遍历过的
                for j in range(4):
                    up = self.plusOne(cur, j)
                    if up not in visited:
                        q.put(up)
                        visited.add(up)
                    down = self.minusOne(cur, j)
                    if down not in visited:
                        q.put(down)
                        visited.add(down)
            # 增加步数
            step += 1
        # 穷举完都没有找到目标密码 直接返回
        return -1

    # s[j]向上拨动一次
    def plusOne(self, s, j):
        ch = list(s)
        if ch[j] == '9':
            ch[j] = '0'
        else:
            ch[j] = str(int(ch[j])+1)
        return ''.join(ch)

    # s[j]向下拨动一次
    def minusOne(self, s, j):
        ch = list(s)
        if ch[j] == '0':
            ch[j] = '9'
        else:
            ch[j] = str(int(ch[j])-1)
        return ''.join(ch)

```

双向BFS优化 看看得了

```
class Solution:
    def openLock(self, deadends: List[str], target: str) -> int:
        # 记录已经穷举过的密码 防止走回头路
        visited = set()
        # 用集合不用队列 可以快速判断元素是否存在
        q1 = set()
        q2 = set()

        step = 0
        q1.add("0000")
        q2.add(target)

        while len(q1) > 0 and len(q2) > 0:
            # 集合在遍历过程中不能修改，用temp存储扩散结果
            temp = set()

            #将q1中的所有节点向周围扩散
            for cur in q1:
                # 判断是否到达终点
                if cur in deadends:
                    continue
                if cur in q2:
                    return step
                visited.add(cur)

                # 将一个节点的未遍历节点加入集合
                for j in range(4):
                    up = self.plusOne(cur,j)
                    if up not in visited:
                        temp.add(up)
                    down = self.minusOne(cur,j)
                    if down not in visited:
                        temp.add(down)
            # 增加步数
            step += 1
            # temp 相当于q1
            # 这里交换q1 q2 下一轮while就是扩散q2
            q1 = q2
            q2 = temp
        return -1
        

    # s[j]向上拨动一次
    def plusOne(self, s, j):
        ch = list(s)
        if ch[j] == '9':
            ch[j] = '0'
        else:
            ch[j] = str(int(ch[j])+1)
        return ''.join(ch)

    # s[j]向下拨动一次
    def minusOne(self, s, j):
        ch = list(s)
        if ch[j] == '0':
            ch[j] = '9'
        else:
            ch[j] = str(int(ch[j])-1)
        return ''.join(ch)

```
