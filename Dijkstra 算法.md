### 二叉树的层级遍历框架
```
import collections
# 输入一棵二叉树的根节点，层序遍历这棵二叉树
def levelTraverse(root):
    if not root:
        return 0
    q = collections.deque()
    q.append(root)

    depth = 1
    # 从上到下遍历二叉树的每一层
    while len(q) > 0:
        sz = len(q)
        # 从左到右遍历每一层的每个节点
        for i in range(sz):
            cur = q.popleft()
            print("节点 %s 在第 %s 层", cur, depth)

            # 将下一层节点放入队列
            if cur.left:
                q.append(cur.left)
            if cur.right:
                q.append(cur.right)
        depth += 1
```

### 二叉树的层级遍历框架 自带depth 不需要for循环
```
import collections

class State:
    def __init__(self, depth, node):
        self.depth = depth
        self.node = node

def levelTraverse(root):
    if not root:
        return 0
    q = collections.deque()
    q.append(State(root, 1))

    # 遍历二叉树的每一个节点
    while len(q) > 0:
        cur = q.popleft()
        cur_node = cur.node
        cur_depth = cur.depth
        print("节点%s在第%s层"%(cur_node, cur_depth))

        # 将子节点放入队列
        if cur_node.left:
            q.append(State(cur_node.left, cur_depth+1))
        if cur_node.right:
            q.append(State(cur_node.right, cur_depth+1))
```            

### 多叉树的层序遍历框架
```
import collections
# 输入一棵多叉树的根节点，层序遍历这棵多叉树
def levelTraverse(root):
    if not root:
        return 0
    q = collections.deque()
    q.append(root)

    depth = 1
    # 从上到下遍历多叉树的每一层
    while len(q) > 0:
        sz = len(q)
        # 从左到右遍历每一层的每个节点
        for i in range(sz):
            cur = q.popleft()
            print("节点 %s 在第 %s 层", cur, depth)

            # 将下一层节点放入队列
            for child in root.children:
                q.append(child)
        depth += 1
```

### BFS（广度优先搜索）的算法框架
```import collections
# 输入起点 进行bfs遍历
def BFS(start):
    # 核心数据结构
    q = collections.deque()
    # 避免走回头路
    visited = []

    # 将起点加入队列
    q.append(start)
    visited.append(start)
    # 记录搜索的步数
    step = 0
    while len(q) > 0:
        sz = len(q)
        # 将当前队列中的所有节点向四周扩散一步
        for i in range(sz):
            cur = q.popleft()
            print("从 %s到%s的最短距离是%s" %(start, cur, step))
        # 将cur的相邻节点加入到队列
            for x in cur.adj():
                if x not in visited:
                    q.append(x)
                    visited.append(x)
        step += 1
```

```
import collections

class State:
    def __init__(self, depth, node):
        self.depth = depth
        self.node = node

def levelTraverse(root):
    if not root:
        return 0
    q = collections.deque()
    q.append(State(root, 1))

    # 遍历二叉树的每一个节点
    while len(q) > 0:
        cur = q.popleft()
        cur_node = cur.node
        cur_depth = cur.depth
        print("节点%s在第%s层"%(cur_node, cur_depth))

        # 将子节点放入队列
        if cur_node.left:
            q.append(State(cur_node.left, cur_depth+1))
        if cur_node.right:
            q.append(State(cur_node.right, cur_depth+1))
```   

### Dijkstra 算法框架  标准 Dijkstra算法 算出start到所有其他节点的最短路径
```
import collections
import queue

class State:
    def __init__(self, ID, distFromStart):
        self.ID = ID
        self.distFromStart = distFromStart

    def __lt__(self, other):
        return self.distFromStart < other.distFromStart

# 返回节点 from 到节点 to 之间的边的权重
def weight(From, To):
    pass

# 输入节点s，返回s的相邻节点
def adj(s):
    pass

# 输入一幅图和一个起点 start，计算 start 到其他节点的最短距离
def dijkstra(start, graph):
    # 图中节点的个数
    V = len(graph)
    # 记录最短路径的权重，可以理解为dp table
    # 定义：distTo[i]的值就是节点start到达节点i的最短路径权重
    # 求最小值 dp table初始化为正无穷
    def get_inf():
        return float('inf')
    distTo = collections.defaultdict(get_inf)
    # base case，start到start的最短距离就是0
    distTo[start] = 0

    # 优先级队列，distFromStart 较小的排在前面
    pq = queue.PriorityQueue()

    # 从起点start开始进行BFS
    pq.put(State(start, 0))

    while pq.qsize > 0:
        curState = pq.get()
        curNodeID = curState.ID
        curDistFromStart = curState.distFromStart

        if curDistFromStart > distTo[curNodeID]:
            # 已经有一条更短的路径到达curNode节点了
            continue
        # 将curNode的相邻节点装入队列
        for nextNodeID in adj(curNodeID):
            # 看看从curNode 到达nextNode的距离是否会更短
            distToNextNode = distTo[curNodeID] + weight(curNodeID, nextNodeID)
            if distTo[nextNodeID] > distToNextNode:
                # 更新dp table
                distTo[nextNodeID] = distToNextNode
                # 将这个节点以及距离放入队列
                pq.put(State(nextNodeID, distToNextNode))
    return distTo
```

### Dijkstra 算法框架  算出start到end的最短路径
```
import collections
import queue

class State:
    def __init__(self, ID, distFromStart):
        self.ID = ID
        self.distFromStart = distFromStart

   def __lt__(self, other):
        return self.distFromStart < other.distFromStart

# 返回节点 from 到节点 to 之间的边的权重
def weight(From, To):
    pass

# 输入节点s，返回s的相邻节点
def adj(s):
    pass

# 输入起点start和终点end,计算起点到终点的最短距离
def dijkstra(start, end, graph):
    # 图中节点的个数
    V = len(graph)
    # 记录最短路径的权重，可以理解为dp table
    # 定义：distTo[i]的值就是节点start到达节点i的最短路径权重
    # 求最小值 dp table初始化为正无穷
    def get_inf():
        return float('inf')
    distTo = collections.defaultdict(get_inf)
    # base case，start到start的最短距离就是0
    distTo[start] = 0

    # 优先级队列，distFromStart 较小的排在前面
    pq = queue.PriorityQueue()

    # 从起点start开始进行BFS
    pq.put(State(start, 0))

    while pq.qsize > 0:
        curState = pq.get()
        curNodeID = curState.ID
        curDistFromStart = curState.distFromStart

        # 加一个判断即可
        if curNodeID == end:
            return curDistFromStart

        if curDistFromStart > distTo[curNodeID]:
            # 已经有一条更短的路径到达curNode节点了
            continue
        # 将curNode的相邻节点装入队列
        for nextNodeID in adj(curNodeID):
            # 看看从curNode 到达nextNode的距离是否会更短
            distToNextNode = distTo[curNodeID] + weight(curNodeID, nextNodeID)
            if distTo[nextNodeID] > distToNextNode:
                # 更新dp table
                distTo[nextNodeID] = distToNextNode
                # 将这个节点以及距离放入队列
                pq.put(State(nextNodeID, distToNextNode))
    # 如果运行到这里，说明从start无法走到end
    return float('inf')
```

### 743. 网络延迟时间
```
import collections
import queue

class State:
    def __init__(self, ID, distFromStart):
        self.ID = ID
        self.distFromStart = distFromStart

    def __lt__(self, other):
        return self.distFromStart < other.distFromStart


# 输入起点start,计算从start到其他节点的最短距离
def dijkstra(start, graph, n):
    # 记录最短路径的权重，可以理解为dp table
    # 定义：distTo[i]的值就是节点start到达节点i的最短路径权重
    # 求最小值 dp table初始化为正无穷
##    def get_inf():
##        return float('inf')
##    distTo = collections.defaultdict(get_inf)
    distTo = {}
    for i in range(1,n+1):
        distTo[i] = float('inf')
    # base case，start到start的最短距离就是0
    distTo[start] = 0

    # 优先级队列，distFromStart 较小的排在前面
    pq = queue.PriorityQueue()

    # 从起点start开始进行BFS
    pq.put(State(start, 0))

    while pq.qsize() > 0:
        curState = pq.get()
        curNodeID = curState.ID
        curDistFromStart = curState.distFromStart

        if curDistFromStart > distTo[curNodeID]:
            # 已经有一条更短的路径到达curNode节点了
            continue
        # 将curNode的相邻节点装入队列
        for neighbor in graph[curNodeID]:
            # 看看从curNode 到达neighbor的距离是否会更短
            nextNodeID = neighbor[0]
            distToNextNode = distTo[curNodeID] + neighbor[1]
            if distTo[nextNodeID] > distToNextNode:
                # 更新dp table
                distTo[nextNodeID] = distToNextNode
                # 将这个节点以及距离放入队列
                pq.put(State(nextNodeID, distToNextNode))
    return distTo

# times 记录边和权重，n 为节点个数（从 1 开始），k 为起点
# 计算从 k 发出的信号至少需要多久传遍整幅图
class Solution:
    def networkDelayTime(self, times: List[List[int]], n: int, k: int) -> int:
        # 节点编号是从1开始的，所以要一个大小为n+1的邻接表
        graph = {}
        for i in range(1, n+1):
            graph[i] = []
        # 构造图
        for edge in times:
            From = edge[0]
            To = edge[1]
            Weight = edge[2]
            # from -> List<(to, weight)>
            # 邻接表存储图结构，同时存储权重信息
            graph[From].append((To, Weight))
        # 启动 dijkstra 算法计算以节点 k 为起点到其他节点的最短路径
        distTo = dijkstra(k, graph, n)

        # 找到最长的那一条最短路径
        res = 0
        for i in range(1, n+1):
            if distTo[i] == float('inf'):
                # 有节点不可达， 返回-1
                return -1
            res = max(res, distTo[i])
        return res
```

### 1631. 最小体力消耗路径
```
import collections
import queue

class State:
    def __init__(self, x, y, effortFromStart):
        # 矩阵中的一个位置
        self.x = x
        self.y = y
        # 从起点 (0, 0) 到当前位置的最小体力消耗（距离）
        self.effortFromStart = effortFromStart

    def __lt__(self, other):
        return self.effortFromStart < other.effortFromStart

# 方向数组 上下左右的坐标偏移量
dirs = [[0,1],[1,0],[0,-1],[-1,0]]


# 返回坐标（x,y)的上下左右相邻坐标
def adj(matrix, x, y):
    m = len(matrix)
    n = len(matrix[0])
    # 存储相邻节点
    neighbors = []
    for Dir in dirs:
        nx = x + Dir[0]
        ny = y + Dir[1]
        if nx >= m or nx < 0 or ny >= n or ny < 0 :
            # 索引越界
            continue
        neighbors.append([nx,ny])
    return neighbors

class Solution:
    # Dijkstra 算法，计算 (0, 0) 到 (m - 1, n - 1) 的最小体力消耗
    def minimumEffortPath(self, heights: List[List[int]]) -> int:
        m = len(heights)
        n = len(heights[0])
        # 定义：从(0,0)到(i,j)的最小体力消耗是effortTo[i][j]
        # dp table 初始化为正无穷
        effortTo = [[float('inf')]*n for _ in range(m)]
        # base case 起点到起点的最小消耗就是0
        effortTo[0][0] = 0

        # 优先级队列 effortFromStart 较小的排在前面
        pq = queue.PriorityQueue()

        # 从起点(0,0)进行BFS
        pq.put(State(0,0,0))

        while pq.qsize() > 0:
            curState = pq.get()
            curX = curState.x
            curY = curState.y
            curEffortFromStart = curState.effortFromStart

            # 到达终点提前结束
            if curX == m - 1 and curY == n - 1:
                return curEffortFromStart
            if curEffortFromStart > effortTo[curX][curY]:
                continue
            # 将(curX, curY)的相邻坐标装入队列
            for neighbor in adj(heights, curX, curY):
                nextX = neighbor[0]
                nextY = neighbor[1]
                # 计算从(curX, curY)达到(nextX, nextY)的消耗
                effortToNextNode = max(effortTo[curX][curY],
                                       abs(heights[curX][curY] -
                                           heights[nextX][nextY]))
                # 更新dp table
                if effortTo[nextX][nextY] > effortToNextNode:
                    effortTo[nextX][nextY] = effortToNextNode
                    pq.put(State(nextX, nextY, effortToNextNode))
        # 正常情况下 不会达到这个return
        return -1
        
```

### 1514. 概率最大的路径
```
import collections
import queue

class State:
    def __init__(self, ID, probFromStart):
        # 图节点的id
        self.ID = ID
        # 从start节点到达当前节点的概率
        self.probFromStart = probFromStart


    def __lt__(self, other):
        return self.probFromStart > other.probFromStart

def dijkstra(start, end, graph, n):
    # 定义：probTo[i]的值就是节点start到达节点i的最大概率
    probTo = {}
    for i in range(n):
        probTo[i] = float('-inf')
    # base case，start到start的概率就是1
    probTo[start] = 1

    # 优先级队列，probFromStart 较大的排在前面
    pq = queue.PriorityQueue()

    # 从起点start开始进行BFS
    pq.put(State(start, 1))

    while pq.qsize() > 0:
        curState = pq.get()
        curNodeID = curState.ID
        curProbFromStart = curState.probFromStart

        # 遇到终点提前返回
        if curNodeID == end:
            return curProbFromStart
        
        if curProbFromStart < probTo[curNodeID]:
            # 已经有一条概率更大的路径到达curNode节点了
            continue
        # 将curNode的相邻节点装入队列
        for neighbor in graph[curNodeID]:
            # 看看从curNode到达的nextNode的概率是否会更大
            nextNodeID = neighbor[0]
            probToNextNode = probTo[curNodeID] * neighbor[1]
            if probTo[nextNodeID] < probToNextNode:
                # 更新dp table
                probTo[nextNodeID] = probToNextNode
                # 把新的点放入堆中
                pq.put(State(nextNodeID, probToNextNode))
    # 如果到达这里 说明从start开始无法到达end,返回0
    return 0

class Solution:
    def maxProbability(self, n: int, edges: List[List[int]], succProb: List[float], start: int, end: int) -> float:
        graph = {}
        for i in range(n):
            graph[i] = []

        # 构造邻接表结构表示图
        for i in range(len(edges)):
            From = edges[i][0]
            To = edges[i][1]
            weight = succProb[i]
            # 无向图就是双向图
            graph[From].append([To,weight])
            graph[To].append([From, weight])

        return dijkstra(start, end, graph, n)
```
