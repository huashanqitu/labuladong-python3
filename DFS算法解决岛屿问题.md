# labuladong-python3

### 200. 岛屿数量

```
class Solution:
    def numIslands(self, grid: List[List[str]]) -> int:
        res = 0
        m = len(grid)
        n = len(grid[0])
        dirs = [[-1,0],[1,0],[0,1],[0,-1]]

        # 从(i, j)开始，将与之相邻的陆地都变成海水
        def dfs(grid, i, j):
            if i < 0 or j < 0 or i >= m or j >= n:
                # 超出索引边界
                return
            if grid[i][j] == '0':
                # 已经是海水
                return
            # 将(i,j)变成海水
            grid[i][j] = '0'
            # 淹没上下左右的陆地
            for d in dirs:
                next_i = i + d[0]
                next_j = j + d[1]
                dfs(grid, next_i, next_j)
            
        
        # 遍历grid
        for i in range(m):
            for j in range(n):
                if grid[i][j] == '1':
                    # 每发现一个岛屿，岛屿数量加一
                    res += 1
                    # 然后使用DFS将岛屿淹了
                    dfs(grid, i, j)
        return res
```      

### 1254. 统计封闭岛屿的数目
```
class Solution:
    def closedIsland(self, grid: List[List[int]]) -> int:
        m = len(grid)
        n = len(grid[0])
        
        # 从（i,j）开始，将与之相邻的陆地都变成海水
        def dfs(grid, i, j):
            if i < 0 or j < 0 or i >= m or j >= n:
                return
            if grid[i][j] == 1:
                # 已经是海水了
                return

            # 将（i,j）变成海水
            grid[i][j] = 1
            # 淹没上下左右的陆地
            dfs(grid, i-1, j)
            dfs(grid, i+1, j)
            dfs(grid, i, j-1)
            dfs(grid, i, j+1)
        
        for j in range(n):
            # 把靠上边的岛屿淹没
            dfs(grid, 0, j)
            # 把靠下边的岛屿淹没
            dfs(grid, m-1, j)

        for i in range(m):
            # 把靠左边的岛屿淹没
            dfs(grid, i, 0)
            # 把靠右边的岛屿淹没
            dfs(grid, i, n-1)

        # 遍历grid,剩下的岛屿都是封闭岛屿
        res = 0
        for i in range(m):
            for j in range(n):
                if grid[i][j] == 0:
                    res += 1
                    dfs(grid, i, j)
        return res
```

### 1020. 飞地的数量
```
class Solution:
    def numEnclaves(self, grid: List[List[int]]) -> int:
        m = len(grid)
        n = len(grid[0])

        # 从（i,j）开始，将与之相邻的陆地都变成海水
        def dfs(grid, i, j):
            if i < 0 or j < 0 or i >= m or j >= n:
                return
            if grid[i][j] == 0:
                # 已经是海水了
                return

            # 将（i,j）变成海水
            grid[i][j] = 0
            # 淹没上下左右的陆地
            dfs(grid, i-1, j)
            dfs(grid, i+1, j)
            dfs(grid, i, j-1)
            dfs(grid, i, j+1)
            
        # 淹没靠边的陆地
        for j in range(n):
            # 把靠上边的岛屿淹没
            dfs(grid, 0, j)
            # 把靠下边的岛屿淹没
            dfs(grid, m-1, j)

        for i in range(m):
            # 把靠左边的岛屿淹没
            dfs(grid, i, 0)
            # 把靠右边的岛屿淹没
            dfs(grid, i, n-1)

        # 数一数剩下的陆地
        res = 0
        for i in range(m):
            for j in range(n):
                if grid[i][j] == 1:
                    res += 1

        return res
```

### 695. 岛屿的最大面积
```
class Solution:
    def maxAreaOfIsland(self, grid: List[List[int]]) -> int:
        # 记录岛屿的最大面积
        res = 0
        m = len(grid)
        n = len(grid[0])

        # 淹没与（i,j）相邻的陆地，并返回淹没的陆地面积
        def dfs(grid, i, j):
            if i < 0 or j < 0 or i >= m or j >= n:
                # 超出索引边界
                return 0
            if grid[i][j] == 0:
                # 已经是海水了
                return 0
            # 将（i,j）变成海水
            grid[i][j] = 0

            return dfs(grid, i + 1, j)+ dfs(grid, i, j + 1)+ dfs(grid, i - 1, j)+ dfs(grid, i, j - 1) + 1
        
        for i in range(m):
            for j in range(n):
                if grid[i][j] == 1:
                    # 淹没岛屿 并更新最大岛屿面积
                    res = max(res, dfs(grid, i, j))
        return res
```

### 1905. 统计子岛屿
```
class Solution:
    def countSubIslands(self, grid1: List[List[int]], grid2: List[List[int]]) -> int:
        m = len(grid1)
        n = len(grid1[0])
        # 从（i,j）开始 与之相邻的陆地都变成海水
        def dfs(grid, i, j):
            m = len(grid)
            n = len(grid[0])
            if i < 0 or j < 0 or i >= m or j>=n:
                return
            if grid[i][j] == 0:
                return

            grid[i][j] = 0
            dfs(grid, i + 1, j)
            dfs(grid, i, j + 1)
            dfs(grid, i - 1, j)
            dfs(grid, i, j - 1)
        
        for i in range(m):
            for j in range(n):
                if grid1[i][j] == 0 and grid2[i][j] == 1:
                    # 这个岛屿肯定不是子岛，淹
                    dfs(grid2, i, j)

        # 现在grid2中剩下的岛屿都是子岛，计算岛屿数量
        res = 0
        for i in range(m):
            for j in range(n):
                if grid2[i][j] == 1:
                    res += 1
                    dfs(grid2, i, j)
        return res
```

### 694. 不同岛屿的数量 此题是leetcode的会员题目 有待验证
```
class Solution:
    def numDistinctIslands(self, grid: List[List[int]]) -> int:
        m = len(grid)
        n = len(grid[0])

        def dfs(grid, i, j, sb, direction):
            if i < 0 or j < 0 or i >= m or j >= n:
                return
            # 前序遍历位置 进入（i,j）
            grid[i][j] = 0
            sb.append(direction)

            # 上下左右
            dfs(grid, i - 1, j, sb, 1)
            dfs(grid, i + 1, j, sb, 2)
            dfs(grid, i, j - 1, sb, 3)
            dfs(grid, i, j + 1, sb, 4)

            # 后序遍历位置 离开（i,j）
            sb.append(-direction)            
        
        # 记录所有岛屿的序列化结果
        islands = set()
        for i in range(m):
            for j in range(n):
                if grid[i][j] == 1:
                    # 淹没这个岛屿， 同时存储岛屿的序列化结果
                    sb = []
                    # 初始方向可以随便写，不影响正确性
                    dfs(grid, i, j, sb, 666)
                    islands.add(','.join([str(k) for k in sb]))
        # 不同岛屿的数量
        return len(islands)
```                        
