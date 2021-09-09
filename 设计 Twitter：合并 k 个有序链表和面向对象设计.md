# labuladong-python3

### 设计推特
```
import time


class Tweet:
    def __init__(self, id, time):
        self.id = id
        self.time = time
        self.next = None

class User:
    def __init__(self, userId):
        self.followed = set()
        self.id = userId
        self.head = None
        # 关注一下自己
        self.follow(self.id)

    def follow(self, userId):
        self.followed.add(userId)

    def unfollow(self, userId):
        # 不可以取关自己  同时要有这个key 不然会报error
        if userId != self.id and userId in self.followed:
            self.followed.remove(userId)

    def post(self, tweetId):
        timestamp = round(time.time()*1000000)
        twt = Tweet(tweetId, timestamp)
        # 将新建的推文插入链表头
        # 越靠前的推文time值越大
        twt.next = self.head
        self.head = twt
    
class Twitter:

    def __init__(self):
        """
        Initialize your data structure here.
        """
        # 我们需要一个映射将userId和User对象对应起来
        self.userMap = {}

    # user发表一条tweet动态
    def postTweet(self, userId: int, tweetId: int) -> None:
        """
        Compose a new tweet.
        """
        # 若userId不存在 则新建
        if userId not in self.userMap:
            self.userMap[userId] = User(userId)
        u = self.userMap.get(userId)
        u.post(tweetId)


    def getNewsFeed(self, userId: int) -> List[int]:
        """
        Retrieve the 10 most recent tweet ids in the user's news feed. Each item in the news feed must be posted by users who the user followed or by the user herself. Tweets must be ordered from most recent to least recent.
        """
        res = []
        if userId not in self.userMap:
            return res
        # 关注列表的用户Id
        users = self.userMap.get(userId).followed
        pq = []
        for id in users:
            twt = self.userMap.get(id).head
            if not twt:
                continue
            pq.append(twt)
        while pq:
            # 最多返回10条就够了
            if len(res) == 10:
                break
            # 弹出time值最大的（最近发表的）
            twt = heapq.nlargest(1, pq, key=lambda x:x.time)[0]
            pq.remove(twt)
            res.append(twt.id)
            # 将下一篇tweet插入进行排序
            if twt.next:
                pq.append(twt.next)
        return res
            

    # follower关注followee
    def follow(self, followerId: int, followeeId: int) -> None:
        """
        Follower follows a followee. If the operation is invalid, it should be a no-op.
        """
        # 若follower不存在，则新建
        if followerId not in self.userMap:
            u = User(followerId)
            self.userMap[followerId] = u
        # 若followee不存在，则新建
        if followeeId not in self.userMap:
            u = User(followeeId)
            self.userMap[followeeId] = u
        self.userMap.get(followerId).follow(followeeId)

    # follower取关followee,如果id不存在则什么都不做
    def unfollow(self, followerId: int, followeeId: int) -> None:
        """
        Follower unfollows a followee. If the operation is invalid, it should be a no-op.
        """
        if followerId in self.userMap:
            flwer = self.userMap.get(followerId)
            flwer.unfollow(followeeId)
        



# Your Twitter object will be instantiated and called as such:
# obj = Twitter()
# obj.postTweet(userId,tweetId)
# param_2 = obj.getNewsFeed(userId)
# obj.follow(followerId,followeeId)
# obj.unfollow(followerId,followeeId)
```
