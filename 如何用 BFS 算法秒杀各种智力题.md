# labuladong-python3

### 773. 滑动谜题
```
from queue import Queue

class Solution:
    def slidingPuzzle(self, board: List[List[int]]) -> int:
        m = 2
        n = 3
        start = ""
        target = "123450"
        # 将2*3的数组转化成字符串
        for i in range(m):
            for j in range(n):
                start += str(board[i][j])

        # 记录一维字符串的相邻索引
        neighbor = {0:[1,3], 1:[0,4,2], 2:[1,5], 3:[0,4], 4:[3,1,5], 5:[4,2]}

        # BFS算法框架开始
        q = Queue()
        visited = {}
        q.put(start)
        visited[start] = True

        step = 0
        while q.qsize() > 0:
            sz = q.qsize()
            for i in range(sz):
                cur = q.get()
                # 判断是否达到目标局面
                if target == cur:
                    return step

                # 找到数字0的索引
                idx = 0
                while cur[idx] != '0':
                    idx += 1
                # 将数字0和相邻的数字交换位置
                for adj in neighbor[idx]:
                    new_board = list(cur)
                    new_board[idx] = new_board[adj]
                    new_board[adj] = '0'
                    new_board = ''.join(new_board)
                    # 防止走回头路
                    if new_board not in visited:
                        q.put(new_board)
                        visited[new_board] = True
            step += 1
        return -1
        
```
