# labuladong-python3

### 1. 两数之和
```
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        n = len(nums)
        index = {}
        # 构造一个哈希表 元素映射到相应的索引
        for i in range(n):
            index[nums[i]] = i

        for i in range(n):
            other = target - nums[i]
            # 如果other存在且不是nums[i]本身
            if other in index and index[other] != i:
                return [i, index[other]]
        return [-1,-1]
```

### 如果数组是有序的 可以用双指针的方法
```
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        left = 0
        right = len(nums)-1
        while left < right:
            sum = nums[left] + nums[right]
            if sum == target:
                return [left, right]
            elif sum < target:
                left += 1
            elif sum > target:
                right -= 1
        return [-1,-1]
```

### TwoSum II
```
from collections import defaultdict

class Solution:
    def __init__(self, nums, target):
        self.nums = nums
        self.target = target
        self.freq = defaultdict(int)


    def add(self, number):
        # 记录number出现次数
        self.freq[number] += 1

    def find(self, value):
        for k in self.freq:
            other = self.target - k
            # 情况一
            if other == k and freq[k] > 1:
                return True
            # 情况二
            if other != k and freq[other] > 0:
                return True
        return False

    
```

### TwoSum II（针对性优化find方法）
```
class Solution:
    def __init__(self, nums, target):
        self.nums = nums
        self.target = target
        self.sum = set()


    def add(self, number):
        # 记录所有可能组成的和
        for n in self.nums:
            self.sum.add(n+number)
        self.nums.append(number)

    def find(self, value):
        if value in self.sum:
            return True
        else:
            return False
```
