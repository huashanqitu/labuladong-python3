# labuladong-python3

### Union-Find 并查集算法 普通版本
```
class UF:
    # 构造函数 n为图的节点总数
    def __init__(self, n):
        # 一开始互不联通
        self.count = n
        # 父节点指针初始指向自己
        self.parent = [i for i in range(n)]

    def union(self, p, q):
        rootP = self.find(p)
        rootQ = self.find(q)
        if rootP == rootQ:
            return
        # 将两棵树合并为一棵
        parent[rootP] = rootQ
        # parent[rootQ] = rootP也是一样
        
        self.count -= 1  # 两个分量合二为一

    # 返回某个节点x的根节点
    def find(self, x):
        # 根节点的parent[x] == x
        while parent[x] != x:
            x = parent[x]
        return x

    # 返回当前的连通分量的个数
    def count(self):
        return self.count


    def connected(self, p, q):
        rootP = find(p)
        rootQ = find(q)
        return rootP == rootQ

    
```

### 平衡性优化  路径压缩
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
        if size[rootP] > size[rootQ]:
            parent[rootQ] = rootP
            size[rootP] += size[rootQ]
        else:
            parent[rootP] = rootQ
            size[rootQ] += size[rootP]
        
        self.count -= 1  # 两个分量合二为一

    # 返回某个节点x的根节点
    def find(self, x):
        # 根节点的parent[x] == x
        while parent[x] != x:
            # 进行路径压缩
            parent[x] = parent[parent[x]]
            x = parent[x]
        return x

    # 返回当前的连通分量的个数
    def count(self):
        return self.count


    def connected(self, p, q):
        rootP = find(p)
        rootQ = find(q)
        return rootP == rootQ

    
```
