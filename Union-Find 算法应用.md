# labuladong-python3

### 130  被围绕的区域
```
class UF:
    # 构造函数 n为图的节点总数
    def __init__(self, n):
        # 一开始互不联通
        self.count = n
        # 父节点指针初始指向自己
        self.parent = [i for i in range(n)]
        # 新增一个数组记录树的重量
        # 最初每棵树只有一个节点 重量应该初始化1
        self.size = [1]*n

    def union(self, p, q):
        rootP = self.find(p)
        rootQ = self.find(q)
        if rootP == rootQ:
            return
        # 小树接到大树下面 较平衡
        if self.size[rootP] > self.size[rootQ]:
            self.parent[rootQ] = rootP
            self.size[rootP] += self.size[rootQ]
        else:
            self.parent[rootP] = rootQ
            self.size[rootQ] += self.size[rootP]
        
        self.count -= 1  # 两个分量合二为一

    # 返回某个节点x的根节点
    def find(self, x):
        # 根节点的parent[x] == x
        while self.parent[x] != x:
            # 进行路径压缩
            self.parent[x] = self.parent[self.parent[x]]
            x = self.parent[x]
        return x

    # 返回当前的连通分量的个数
    def count(self):
        return self.count


    def connected(self, p, q):
        rootP = self.find(p)
        rootQ = self.find(q)
        return rootP == rootQ

    



class Solution:
    def solve(self, board: List[List[str]]) -> None:
        """
        Do not return anything, modify board in-place instead.
        """
        if len(board) == 0:
            return
        m = len(board)
        n = len(board[0])
        # 给dummy留一个额外位置
        uf = UF(m*n+1)
        dummy = m*n
        # 将首列和末列的0与dummy联通
        for i in range(m):
            if board[i][0] == 'O':
                uf.union(i*n , dummy)
            if board[i][n-1] == 'O':
                uf.union(i*n+n-1, dummy)
        # 将首行和末行的O与dummy联通
        for j in range(n):
            if board[0][j] == 'O':
                uf.union(j, dummy)
            if board[m-1][j] == 'O':
                uf.union((m-1)*n+j, dummy)
        # 方向数组d是上下左右搜索的常用方法
        d = [[1,0],[0,1],[0,-1],[-1,0]]
        for i in range(1, m-1):
            for j in range(1, n-1):
                if board[i][j] == 'O':
                    # 将此O与上下左右的O联通
                    for k in range(4):
                        x = i + d[k][0]
                        y = j + d[k][1]
                        if board[x][y] == 'O':
                            uf.union(x*n+y, i*n+j)
        # 所有不和dummy连通的O 都要被替换
        for i in range(1,m-1):
            for j in range(1,n-1):
                if not uf.connected(dummy, i*n+j):
                    board[i][j] = 'X'
        
```

### 判定合法算式
```
class UF:
    # 构造函数 n为图的节点总数
    def __init__(self, n):
        # 一开始互不联通
        self.count = n
        # 父节点指针初始指向自己
        self.parent = [i for i in range(n)]
        # 新增一个数组记录树的重量
        # 最初每棵树只有一个节点 重量应该初始化1
        self.size = [1]*n

    def union(self, p, q):
        rootP = self.find(p)
        rootQ = self.find(q)
        if rootP == rootQ:
            return
        # 小树接到大树下面 较平衡
        if self.size[rootP] > self.size[rootQ]:
            self.parent[rootQ] = rootP
            self.size[rootP] += self.size[rootQ]
        else:
            self.parent[rootP] = rootQ
            self.size[rootQ] += self.size[rootP]
        
        self.count -= 1  # 两个分量合二为一

    # 返回某个节点x的根节点
    def find(self, x):
        # 根节点的parent[x] == x
        while self.parent[x] != x:
            # 进行路径压缩
            self.parent[x] = self.parent[self.parent[x]]
            x = self.parent[x]
        return x

    # 返回当前的连通分量的个数
    def count(self):
        return self.count


    def connected(self, p, q):
        rootP = self.find(p)
        rootQ = self.find(q)
        return rootP == rootQ

    
class Solution:
    def equationsPossible(self, equations):
        # 26个英文字母
        uf = UF(26)
        # 先让相等的字母形成连通分量
        for eq in equations:
            if eq[1] == '=':
                x = eq[0]
                y = eq[3]
                uf.union(ord(x)-ord('a'), ord(y)-ord('a'))
        # 检查不等关系是否打破相等关系的连通性
        for eq in equations:
            if eq[1] == '!':
                x = eq[0]
                y = eq[3]
                # 如果相等关系成立 就是逻辑冲突
                if uf.connected(ord(x)-ord('a'), ord(y)-ord('a')):
                    return False
        return True
```
