# labuladong-python3

### 931. 下降路径最小和 暴力穷举
```
class Solution:
    def minFallingPathSum(self, matrix: List[List[int]]) -> int:
        n = len(matrix)
        res = float('inf')
        for j in range(n):
            res = min(res, self.dp(matrix,n-1,j))
        return res

    # 暴力穷举
    def dp(self, matrix, i, j):
        # 非法索引检查
        if i < 0 or j < 0 or j >= len(matrix) or i >= len(matrix):
            # 返回一个特殊值
            return 99999

        # base case
        if i == 0:
            return matrix[i][j]

        # 状态转移
        return matrix[i][j] + min(self.dp(matrix,i-1,j),\
                                  self.dp(matrix,i-1,j-1),\
                                  self.dp(matrix,i-1,j+1))

    
```

### 931. 下降路径最小和 备忘录
```
class Solution:
    def __init__(self):
        # 备忘录里的值初始化为66666
        self.memo = None

    def minFallingPathSum(self, matrix: List[List[int]]) -> int:
        n = len(matrix)
        self.memo = [[66666]*n for _ in range(n)]
        res = float('inf')
        # 终点可能在matrix[n-1]的任意一列
        for j in range(n):
            res = min(res, self.dp(matrix,n-1,j))
        return res

    def dp(self, matrix, i, j):
        # 索引合法性检查
        if i < 0 or j < 0 or i >= len(matrix) or j >= len(matrix):
            return 99999

        # base case
        if i == 0:
            return matrix[0][j]

        # 查找备忘录 防止重复计算
        if self.memo[i][j] != 66666:
            return self.memo[i][j]

        # 进行状态转移
        self.memo[i][j] = matrix[i][j] + min(self.dp(matrix,i-1,j),\
                                        self.dp(matrix,i-1,j-1),\
                                        self.dp(matrix,i-1,j+1))

        return self.memo[i][j]

```
