# Assignment #A: 图论：算法，树算及栈

Updated 2018 GMT+8 Apr 21, 2024

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



## 1. 题目

### 20743: 整人的提词本

http://cs101.openjudge.cn/practice/20743/



思路：



代码

```python
def reverse_parentheses(s):
    stack = []
    for char in s:
        if char == ')':
            temp = []
            while stack and stack[-1] != '(':
                temp.append(stack.pop())
            # remove the opening parenthesis
            if stack:
                stack.pop()
            # add the reversed characters back to the stack
            stack.extend(temp)
        else:
            stack.append(char)
    return ''.join(stack)

s = input().strip()
print(reverse_parentheses(s))
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240430111017505](C:\Users\86133\AppData\Roaming\Typora\typora-user-images\image-20240430111017505.png)



### 02255: 重建二叉树

http://cs101.openjudge.cn/practice/02255/



思路：



代码

```python
def build_tree(preorder, inorder):
    if not preorder:
        return ''
    
    root = preorder[0]
    root_index = inorder.index(root)
    
    left_preorder = preorder[1:1 + root_index]
    right_preorder = preorder[1 + root_index:]
    
    left_inorder = inorder[:root_index]
    right_inorder = inorder[root_index + 1:]
    
    left_tree = build_tree(left_preorder, left_inorder)
    right_tree = build_tree(right_preorder, right_inorder)
    
    return left_tree + right_tree + root

while True:
    try:
        preorder, inorder = input().split()
        postorder = build_tree(preorder, inorder)
        print(postorder)
    except EOFError:
        break
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240430111112715](C:\Users\86133\AppData\Roaming\Typora\typora-user-images\image-20240430111112715.png)



### 01426: Find The Multiple

http://cs101.openjudge.cn/practice/01426/

要求用bfs实现



思路：



代码

```python
from collections import deque

def find_multiple(n):
    q = deque()
    q.append((1 % n, "1"))
    visited = set([1 % n])  

    while q:
        mod, num_str = q.popleft()
        if mod == 0:
            return num_str

        for digit in ["0", "1"]:
            new_num_str = num_str + digit
            new_mod = (mod * 10 + int(digit)) % n

            if new_mod not in visited:
                q.append((new_mod, new_num_str))
                visited.add(new_mod)

def main():
    while True:
        n = int(input())
        if n == 0:
            break
        print(find_multiple(n))

if __name__ == "__main__":
    main()
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240430111247915](C:\Users\86133\AppData\Roaming\Typora\typora-user-images\image-20240430111247915.png)



### 04115: 鸣人和佐助

bfs, http://cs101.openjudge.cn/practice/04115/



思路：



代码

```python
from collections import deque

m, n, chakra = map(int, input().split())
graph = []
pos = set()
ans = []

naruto = sasuke = None
for i in range(m):
    arr = list(input())
    if '@' in arr:
        naruto = (i, arr.index('@'))
        pos.add((naruto[0], naruto[1], chakra))
    if '+' in arr:
        sasuke = (i, arr.index('+'))

    graph.append(arr)

graph[sasuke[0]][sasuke[1]] = '*'

stepx = [-1, 1, 0, 0]
stepy = [0, 0, -1, 1]


def bfs():
    global ans
    queue = deque()
    queue.append((naruto[0], naruto[1], 0, chakra))


    while queue:
        x, y, depth, chak = queue.popleft()

        if (x, y) == sasuke:
            ans.append(depth)
            return


        for i in range(4):
            x1, y1 = x + stepx[i], y + stepy[i]
            if 0 <= x1 < m and 0 <= y1 < n :
                if graph[x1][y1] == '*' and (x1, y1, chak) not in pos:
                    queue.append((x1, y1, depth + 1, chak))
                    pos.add((x1, y1, chak))

                elif chak > 0 and graph[x1][y1] == '#' and (x1, y1, chak-1) not in pos:
                    queue.append((x1, y1, depth + 1, chak-1))
                    pos.add((x1, y1, chak-1))

bfs()
if ans:
    print(min(ans))
else:
    print(-1)# 

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240430113200109](C:\Users\86133\AppData\Roaming\Typora\typora-user-images\image-20240430113200109.png)



### 20106: 走山路

Dijkstra, http://cs101.openjudge.cn/practice/20106/



思路：



代码

```python
#heap局部最优
from heapq import heappop, heappush

def bfs(x1, y1):
    q = [(0, x1, y1)]
    v = set()
    while q:
        t, x, y = heappop(q)
        v.add((x, y))
        if x == x2 and y == y2:
            return t
        for dx, dy in dir:
            nx, ny = x+dx, y+dy
            if 0 <= nx < m and 0 <= ny < n and ma[nx][ny] != '#' and (nx, ny) not in v:
                nt = t+abs(int(ma[nx][ny])-int(ma[x][y]))
                heappush(q, (nt, nx, ny))
    return 'NO'


m, n, p = map(int, input().split())
ma = [list(input().split()) for _ in range(m)]
dir = [(1, 0), (-1, 0), (0, 1), (0, -1)]
for _ in range(p):
    x1, y1, x2, y2 = map(int, input().split())
    if ma[x1][y1] == '#' or ma[x2][y2] == '#':
        print('NO')
        continue
    print(bfs(x1, y1))
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240430111608438](C:\Users\86133\AppData\Roaming\Typora\typora-user-images\image-20240430111608438.png)



### 05442: 兔子与星空

Prim, http://cs101.openjudge.cn/practice/05442/



思路：



代码

```python
import heapq

def prim(graph, start):
    mst = []
    used = set([start])
    edges = [
        (cost, start, to)
        for to, cost in graph[start].items()
    ]
    heapq.heapify(edges)

    while edges:
        cost, frm, to = heapq.heappop(edges)
        if to not in used:
            used.add(to)
            mst.append((frm, to, cost))
            for to_next, cost2 in graph[to].items():
                if to_next not in used:
                    heapq.heappush(edges, (cost2, to, to_next))

    return mst

def solve():
    n = int(input())
    graph = {chr(i+65): {} for i in range(n)}
    for i in range(n-1):
        data = input().split()
        star = data[0]
        m = int(data[1])
        for j in range(m):
            to_star = data[2+j*2]
            cost = int(data[3+j*2])
            graph[star][to_star] = cost
            graph[to_star][star] = cost
    mst = prim(graph, 'A')
    print(sum(x[2] for x in mst))

solve()
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240430111445866](C:\Users\86133\AppData\Roaming\Typora\typora-user-images\image-20240430111445866.png)



## 2. 学习总结和收获

==如果作业题目简单，有否额外练习题目，比如：OJ“2024spring每日选做”、CF、LeetCode、洛谷等网站题目。==一些上学期的题目记忆犹新~，bfs和dfs的掌握程度比树要好一些，准备回去重新理一下树的脉络



