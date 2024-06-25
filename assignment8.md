# Assignment #8: 图论：概念、遍历，及 树算

Updated 1919 GMT+8 Apr 8, 2024

2024 spring, Complied by ==生命科学学院 施广熠==



**说明：**

1）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

2）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

3）如果不能在截止前提交作业，请写明原因。



**编程环境**

==（请改为同学的操作系统、编程环境等）==

操作系统：windous11

Python编程环境：PyCharm 2023.1.4 (Professional Edition)

C/C++编程环境：无



## 1. 题目

### 19943: 图的拉普拉斯矩阵

matrices, http://cs101.openjudge.cn/practice/19943/

请定义Vertex类，Graph类，然后实现



思路：计概方法——用邻接矩阵表示图，一列一列的改变A。

数算方法——定义Vertex和Graph类，然后定义构建拉普拉斯矩阵的函数。



代码

```python
n,m=map(int,input().split())
A=[[0]*n for _ in range(n)]#注意生成矩阵的方式
#生成矩阵A
for _ in range(m):
    i,j=map(int,input().split())
    A[i][j]=A[j][i]=1
#计算D-A,一列一列的改变A，不会影响结果
for column in range(n):
    sumup=0
    for line in range(n):
        sumup+=A[line][column]
        A[line][column]=-A[line][column]
    A[column][column]+=sumup
    
for lines in A:
    print(' '.join([str(x) for x in lines]))
```

```python
class Vertex:	
    def __init__(self, key):
        self.id = key
        self.connectedTo = {}

    def addNeighbor(self, nbr, weight=0):
        self.connectedTo[nbr] = weight

    def __str__(self):
        return str(self.id) + ' connectedTo: ' + str([x.id for x in self.connectedTo])

    def getConnections(self):
        return self.connectedTo.keys()

    def getId(self):
        return self.id

    def getWeight(self, nbr):
        return self.connectedTo[nbr]

class Graph:
    def __init__(self):
        self.vertList = {}
        self.numVertices = 0

    def addVertex(self, key):
        self.numVertices = self.numVertices + 1
        newVertex = Vertex(key)
        self.vertList[key] = newVertex
        return newVertex

    def getVertex(self, n):
        if n in self.vertList:
            return self.vertList[n]
        else:
            return None

    def __contains__(self, n):
        return n in self.vertList

    def addEdge(self, f, t, weight=0):
        if f not in self.vertList:
            nv = self.addVertex(f)
        if t not in self.vertList:
            nv = self.addVertex(t)
        self.vertList[f].addNeighbor(self.vertList[t], weight)

    def getVertices(self):
        return self.vertList.keys()

    def __iter__(self):
        return iter(self.vertList.values())

def constructLaplacianMatrix(n, edges):
    graph = Graph()
    for i in range(n):	# 添加顶点
        graph.addVertex(i)
    
    for edge in edges:	# 添加边
        a, b = edge
        graph.addEdge(a, b)
        graph.addEdge(b, a)
    
    laplacianMatrix = []	# 构建拉普拉斯矩阵
    for vertex in graph:
        row = [0] * n
        row[vertex.getId()] = len(vertex.getConnections())
        for neighbor in vertex.getConnections():
            row[neighbor.getId()] = -1
        laplacianMatrix.append(row)

    return laplacianMatrix


n, m = map(int, input().split())	# 解析输入
edges = []
for i in range(m):
    a, b = map(int, input().split())
    edges.append((a, b))

laplacianMatrix = constructLaplacianMatrix(n, edges)	# 构建拉普拉斯矩阵

for row in laplacianMatrix:	# 输出结果
    print(' '.join(map(str, row)))
```



代码运行截图 ==（至少包含有"Accepted"）==

![d2cbfd7ff1372c1c7aada204bc180de](C:\Users\86133\Documents\WeChat Files\wxid_r99ulkp2yilm12\FileStorage\Temp\d2cbfd7ff1372c1c7aada204bc180de.png)

### 18160: 最大连通域面积

matrix/dfs similar, http://cs101.openjudge.cn/practice/18160



思路：思路是dfs，从一个点向四面八方dfs。visit数组用`matrix[x][y]='.'`替代，似乎更方便一点。



代码

```python
# 
dire = [[-1,-1],[-1,0],[-1,1],[0,-1],[0,1],[1,-1],[1,0],[1,1]]

area = 0
def dfs(x,y):
    global area
    if matrix[x][y] == '.':return
    matrix[x][y] = '.'
    area += 1
    for i in range(len(dire)):
        dfs(x+dire[i][0], y+dire[i][1])


for _ in range(int(input())):
    n,m = map(int,input().split())

    matrix = [['.' for _ in range(m+2)] for _ in range(n+2)]
    for i in range(1,n+1):
        matrix[i][1:-1] = input()

    sur = 0
    for i in range(1, n+1):
        for j in range(1, m+1):
            if matrix[i][j] == 'W':
                area = 0 
                dfs(i, j)
                sur = max(sur, area)
    print(sur)
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240416213200358](C:\Users\86133\AppData\Roaming\Typora\typora-user-images\image-20240416213200358.png)

### sy383: 最大权值连通块

https://sunnywhy.com/sfbj/10/3/383



思路：学习题解的代码书写。DFS，从一个节点开始，访问它的每一个邻居。可以使用一个visited数组来跟踪每个节点是否已经被访问过。对于每个连通块，可以计算其权值之和，并更新最大权值。

有一点惊讶就是把def dfs写在了def max_weight函数的内部，这样写竟然是可以允许的。



代码

```python
def max_weight(n, m, weights, edges):
    graph = [[] for _ in range(n)]
    for u, v in edges:
        graph[u].append(v)
        graph[v].append(u)

    visited = [False] * n
    max_weight = 0

    def dfs(node):
        visited[node] = True
        total_weight = weights[node]
        for neighbor in graph[node]:
            if not visited[neighbor]:
                total_weight += dfs(neighbor)
        return total_weight

    for i in range(n):
        if not visited[i]:
            max_weight = max(max_weight, dfs(i))

    return max_weight

# 接收数据
n, m = map(int, input().split())
weights = list(map(int, input().split()))
edges = []
for _ in range(m):
    u, v = map(int, input().split())
    edges.append((u, v))

print(max_weight(n, m, weights, edges))

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![50815ec05f384581a0b023d89ce59a6](C:\Users\86133\Documents\WeChat Files\wxid_r99ulkp2yilm12\FileStorage\Temp\50815ec05f384581a0b023d89ce59a6.png)



### 03441: 4 Values whose Sum is 0

data structure/binary search, http://cs101.openjudge.cn/practice/03441



思路：首先创建一个字典来储存数对的数量和频率，然后找相加为0的组合。



代码

```python
from collections import defaultdict

def four_sum_count(A, B, C, D):
    # Create a hash map to store the sum of pairs and their frequencies
    sum_pairs = defaultdict(int)
    for a in A:
        for b in B:
            sum_pairs[a + b] += 1

    # Find the quadruplets with a sum of zero
    count = 0
    for c in C:
        for d in D:
            if -(c + d) in sum_pairs:
                count += sum_pairs[-(c + d)]

    return count
n=int(input())
A=[]
B=[]
C=[]
D=[]
for _ in range(n):
    a,b,c,d=map(int,input().split())
    A.append(a)
    B.append(b)
    C.append(c)
    D.append(d)

# Compute the number of quadruplets
print(four_sum_count(A, B, C, D))

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![caa81db772bc305a919200182dd6c88](C:\Users\86133\Documents\WeChat Files\wxid_r99ulkp2yilm12\FileStorage\Temp\caa81db772bc305a919200182dd6c88.png)

### 04089: 电话号码

trie, http://cs101.openjudge.cn/practice/04089/

Trie 数据结构可能需要自学下。



思路：



代码

```python
class Solution:
    def is_consistent(self, phone_numbers):
        phone_numbers.sort()  
        for i in range(len(phone_numbers) - 1):
            if phone_numbers[i + 1].startswith(phone_numbers[i]):
                return False
        return True

def main():
    t = int(input().strip())
    for _ in range(t):
        n = int(input().strip())
        phone_numbers = [input().strip() for _ in range(n)]
        solution = Solution()
        if solution.is_consistent(phone_numbers):
            print("YES")
        else:
            print("NO")

if __name__ == "__main__":
    main()
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240416213815439](C:\Users\86133\AppData\Roaming\Typora\typora-user-images\image-20240416213815439.png)



### 04082: 树的镜面映射

http://cs101.openjudge.cn/practice/04082/



思路：



代码

```python
class binarynode:
    def __init__(self, value):
        self.value = value
        self.left = None
        self.right = None
        self.children = []
        self.parent = None

n = int(input())
lst = input().split()
stack = []
nodes = []
for x in lst:
    temp = binarynode(x[0])
    nodes.append(temp)
    if stack:
        if stack[-1].left:
            stack[-1].right = temp
            stack.pop()
        else:
            stack[-1].left = temp
    if x[1] == "0":
        stack.append(temp)

for x in nodes:
    if x.left and x.left.value != "$":
        x.children.append(x.left)
        x.left.parent = x
    if x.right and x.right.value != "$":
        x.parent.children.append(x.right)
        x.right.parent = x.parent

for x in nodes:
    x.children = x.children[::-1]

lst1 = [nodes[0]]
for x in lst1:
    if x.children:
        lst1 += x.children
print(" ".join([x.value for x in lst1]))
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240416213953252](C:\Users\86133\AppData\Roaming\Typora\typora-user-images\image-20240416213953252.png)



## 2. 学习总结和收获

==如果作业题目简单，有否额外练习题目，比如：OJ“2024spring每日选做”、CF、LeetCode、洛谷等网站题目。==

复习了上学期的内容，这学期结合树相关的知识，又有新的收获，比较了一些两次代码的差异，对于原来的题型可以有新的写法。



