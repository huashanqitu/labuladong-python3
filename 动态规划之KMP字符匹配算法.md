# labuladong-python3

### 28 实现 strStr()
```
class Solution:
    def strStr(self, haystack: str, needle: str) -> int:
        if not needle:
            return 0
        pat = needle
        txt = haystack
        M = len(pat)
        dp = [[0 for _ in range(256)] for _ in pat]
        dp[0][ord(pat[0])] = 1
        X = 0
        for j in range(1,M):
            for c in range(256):
                if ord(pat[j]) == c:
                    dp[j][c] = j + 1
                else:
                    dp[j][c] = dp[X][c]
            # 更新影子状态
            X = dp[X][ord(pat[j])]
            
        N = len(txt)
        j = 0
        for i in range(N):
            j = dp[j][ord(txt[i])]
            if j == M:
                return i - M + 1
        return -1
```        
