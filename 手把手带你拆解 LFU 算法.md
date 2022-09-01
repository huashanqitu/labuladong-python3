# labuladong-python3

### 460 LFU缓存
```
class LFUCache:

    def __init__(self, capacity: int):
        self.keyToVal = {}
        self.keyToFreq = {}
        self.freqToKeys = {}
        self.cap = capacity
        self.minFreq = 0


    def get(self, key: int) -> int:
        if key not in self.keyToVal:
            return -1
        # 增加key对应的freq
        self.increaseFreq(key)
        return self.keyToVal.get(key)


    def put(self, key: int, val: int) -> None:
        if self.cap <=0:
            return
        # 若key已存在，修改对应的val即可
        if key in self.keyToVal:
            self.keyToVal[key] = val
            # key对应的freq加一
            self.increaseFreq(key)
            return

        # key不存在 需要插入
        # 容量已满的话需要淘汰一个freq最小的key
        if len(self.keyToVal) >= self.cap:
            self.removeMinFreqKey()

        # 插入key和val,对应的freq为1
        # 插入KV表
        self.keyToVal[key] = val
        # 插入KF表
        self.keyToFreq[key] = 1
        # 插入FK表
        if 1 not in self.freqToKeys:
            self.freqToKeys[1] = [key]
        else:
            self.freqToKeys[1].append(key)
        # 插入新key后 最小的freq肯定是1
        self.minFreq = 1

    def removeMinFreqKey(self):
        # freq最小的key列表
        keyList = self.freqToKeys.get(self.minFreq)
        # 其中最先被插入的那个key就是该被淘汰的key
        deletedKey = keyList[0]
        # 更新FK表
        keyList.remove(deletedKey)
        if not keyList:
            self.freqToKeys.pop(self.minFreq)
        # 更新KV表
        self.keyToVal.pop(deletedKey)
        # 更新KF表
        self.keyToFreq.pop(deletedKey)

    def increaseFreq(self, key):
        freq = self.keyToFreq.get(key)
        # 更新KF表
        self.keyToFreq[key] = freq + 1
        # 更新FK表
        # 将key从freq对应的列表中删除
        self.freqToKeys.get(freq).remove(key)
        # 将key加入freq+1对应的列表中
        if freq+1 in self.freqToKeys:
            self.freqToKeys.get(freq+1).append(key)
        else:
            self.freqToKeys[freq+1] = [key]
        # 如果freq对应的列表空了 移除这个freq
        if self.freqToKeys.get(freq) == []:
            self.freqToKeys.pop(freq)
            # 如果这个freq恰好是minFreq, 更新minFreq
            if freq == self.minFreq:
                self.minFreq += 1
        



# Your LFUCache object will be instantiated and called as such:
# obj = LFUCache(capacity)
# param_1 = obj.get(key)
# obj.put(key,value)
```
