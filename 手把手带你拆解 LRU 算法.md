# labuladong-python3

### LRU 缓存机制
```
class Node:
    def __init__(self, key, val, next=None, prev=None):
        self.key = key
        self.val = val
        self.next = next
        self.prev = prev

class DoubleList:
    def __init__(self):
        # 初始化双向链表的数据  包含头尾虚节点  链表元素数
        self.head = Node(0,0)
        self.tail = Node(0,0)
        self.head.next = self.tail
        self.tail.prev = self.head
        self.size = 0

    # 在链表尾部添加节点x,时间O(1)
    def addLast(self, x):
        x.prev = self.tail.prev
        x.next = self.tail
        self.tail.prev.next = x
        self.tail.prev = x
        self.size += 1

    # 删除链表中的x节点（x 一定存在）
    # 由于是双链表且给的是目标Node节点，时间O(1)
    def remove(self, x):
        x.prev.next = x.next
        x.next.prev = x.prev
        self.size -= 1


    # 删除链表中第一个节点，并返回该节点，时间O(1)
    def removeFirst(self):
        if self.head.next == self.tail:
            return None
        first = self.head.next
        self.remove(first)
        return first

    # 返回链表长度 时间O(1)
    def size(self):
        return self.size


class LRUCache:

    def __init__(self, capacity: int):
        self.cap = capacity
        self.map = {}
        self.cache = DoubleList()

    def get(self, key):
        if key not in self.map:
            return -1
        # 将该数据提升为最近使用的
        self.makeRecently(key)
        return self.map.get(key).val

    def put(self, key, val):
        if key in self.map:
            # 删除旧的数据
            self.deleteKey(key)
            # 新插入的数据为最近使用的数据
            self.addRecently(key, val)
            return
        if self.cap == self.cache.size:
            # 删除最久未使用的元素
            self.removeLeastRecently()
        self.addRecently(key, val)

    # 将某个key提升为最近使用的
    def makeRecently(self, key):
        x = self.map.get(key)
        # 先从链表中删除这个节点
        self.cache.remove(x)
        # 重新插到队尾
        self.cache.addLast(x)

    # 添加最近使用的元素
    def addRecently(self, key, val):
        x = Node(key, val)
        # 链表尾部就是最近使用的元素
        self.cache.addLast(x)
        # 在map中添加key的映射
        self.map[key] = x

    # 删除某一个key
    def deleteKey(self, key):
        x = self.map.get(key)
        # 从链表中删除
        self.cache.remove(x)
        # 从map中删除
        self.map.pop(key)

    # 删除最久未使用的元素
    def removeLeastRecently(self):
        # 链表头部的第一个元素就是最久未使用的
        deletedNode = self.cache.removeFirst()
        # 同时别忘了从map中删除它的key
        deletedKey = deletedNode.key
        self.map.pop(deletedKey)



# Your LRUCache object will be instantiated and called as such:
# obj = LRUCache(capacity)
# param_1 = obj.get(key)
# obj.put(key,value)
        
```
