# Assignment #7: April 月考

Updated 1557 GMT+8 Apr 3, 2024

2024 spring, Complied by ==施广熠 生命科学学院==



**说明：**

1）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

2）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

3）如果不能在截止前提交作业，请写明原因。



**编程环境**

==（请改为同学的操作系统、编程环境等）==

操作系统：windous11

Python编程环境： PyCharm 2023.1.4 (Professional Edition)

C/C++编程环境：无

### 27706: 逐词倒放

http://cs101.openjudge.cn/practice/27706/



思路：



代码

```python
l=input().split()
l.reverse()
print(*l)
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240409230536288](C:\Users\86133\AppData\Roaming\Typora\typora-user-images\image-20240409230536288.png)



### 27951: 机器翻译

http://cs101.openjudge.cn/practice/27951/



思路：



代码

```python
from collections import deque

M, N = map(int, input().split())
words = list(map(int, input().split()))

memory = deque()
lookups = 0

for word in words:
    if word not in memory:
        if len(memory) == M:
            memory.popleft()
        memory.append(word)
        lookups += 1

print(lookups)
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240409231102367](C:\Users\86133\AppData\Roaming\Typora\typora-user-images\image-20240409231102367.png)



### 27932: Less or Equal

http://cs101.openjudge.cn/practice/27932/



思路：



代码

```python
n, k = map(int, input().split())

a = list(map(int, input().split()))
a.sort()
if k == 0:
    x = 1 if a[0] > 1 else -1
elif k == n:
    x = a[-1]
else:
    x = a[k-1] if a[k-1] < a[k] else -1

print(x)
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240409231424833](C:\Users\86133\AppData\Roaming\Typora\typora-user-images\image-20240409231424833.png)



### 27948: FBI树

http://cs101.openjudge.cn/practice/27948/



思路：



代码

```python
class Node:
    def __init__(self):
        self.value = None
        self.left = None
        self.right = None

def build_FBI(string):
    root = Node()
    if '0' not in string:
        root.value = 'I'
    elif '1' not in string:
        root.value = 'B'
    else:
        root.value = 'F'
    l = len(string) // 2
    if l > 0:
        root.left = build_FBI(string[:l])
        root.right = build_FBI(string[l:])
    return root

def post_traverse(node):
    ans = []
    if node:
        ans.extend(post_traverse(node.left))
        ans.extend(post_traverse(node.right))
        ans.append(node.value)
    return ''.join(ans)

n = int(input())
string = input()
root = build_FBI(string)
print(post_traverse(root))
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240409231602460](C:\Users\86133\AppData\Roaming\Typora\typora-user-images\image-20240409231602460.png)



### 27925: 小组队列

http://cs101.openjudge.cn/practice/27925/



思路：



代码

```python
from collections import deque	

# Initialize groups and mapping of members to their groups
t = int(input())
groups = {}
member_to_group = {}



for _ in range(t):
    members = list(map(int, input().split()))
    group_id = members[0]  # Assuming the first member's ID represents the group ID
    groups[group_id] = deque()
    for member in members:
        member_to_group[member] = group_id

# Initialize the main queue to keep track of the group order
queue = deque()
# A set to quickly check if a group is already in the queue
queue_set = set()


while True:
    command = input().split()
    if command[0] == 'STOP':
        break
    elif command[0] == 'ENQUEUE':
        x = int(command[1])
        group = member_to_group.get(x, None)
        # Create a new group if it's a new member not in the initial list
        if group is None:
            group = x
            groups[group] = deque([x])
            member_to_group[x] = group
        else:
            groups[group].append(x)
        if group not in queue_set:
            queue.append(group)
            queue_set.add(group)
    elif command[0] == 'DEQUEUE':
        if queue:
            group = queue[0]
            x = groups[group].popleft()
            print(x)
            if not groups[group]:  # If the group's queue is empty, remove it from the main queue
                queue.popleft()
                queue_set.remove(group)
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240409231741887](C:\Users\86133\AppData\Roaming\Typora\typora-user-images\image-20240409231741887.png)



### 27928: 遍历树

http://cs101.openjudge.cn/practice/27928/



思路：



代码

```python
from collections import defaultdict
n = int(input())
tree = defaultdict(list)
parents = []
children = []
for i in range(n):
    t = list(map(int, input().split()))
    parents.append(t[0])
    if len(t) > 1:
        ch = t[1::]
        children.extend(ch)
        tree[t[0]].extend(ch)


def traversal(node):
    seq = sorted(tree[node] + [node])
    for x in seq:
        if x == node:
            print(node)
        else:
            traversal(x)


traversal((set(parents) - set(children)).pop())
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240409231916686](C:\Users\86133\AppData\Roaming\Typora\typora-user-images\image-20240409231916686.png)



## 2. 学习总结和收获

==如果作业题目简单，有否额外练习题目，比如：OJ“2024spring每日选做”、CF、LeetCode、洛谷等网站题目。==

临近期中，数算的题目难度有所下降，可以多关注笔试的题目，虽然期末对笔试的没有特别高的要求，但理论基础还需关注



