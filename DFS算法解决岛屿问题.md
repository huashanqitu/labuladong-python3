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
