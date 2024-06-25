# Assignment #2: 编程练习

Updated 0953 GMT+8 Feb 24, 2024

2024 spring, Complied by ==施广熠 生命科学学院==



**说明：**

1）The complete process to learn DSA from scratch can be broken into 4 parts:
- Learn about Time and Space complexities
- Learn the basics of individual Data Structures
- Learn the basics of Algorithms
- Practice Problems on DSA

2）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）课程网站是Canvas平台, https://pku.instructure.com, 学校通知3月1日导入选课名单后启用。**作业写好后，保留在自己手中，待3月1日提交。**

提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

4）如果不能在截止前提交作业，请写明原因。



**编程环境**

==（请改为同学的操作系统、编程环境等）==

操作系统：windous11

Python编程环境：PyCharm 2023.1.4 (Professional Edition)

C/C++编程环境：无



## 1. 题目

### 27653: Fraction类

http://cs101.openjudge.cn/2024sp_routine/27653/



思路：



##### 代码

```python
def gcd(m,n):
    while m%n != 0:
        oldm = m
        oldn = n
        m = oldn
        n = oldm%oldn
    return n
class fra:
    def __init__(self,top,bottom):
        com=gcd(top,bottom)
        self.top = top//com
        self.bot = bottom//com
    def __str__(self):
        return str(self.top)+"/"+str(self.bot)
    def __add__(self, other):
        return fra(self.top*other.bot+self.bot*other.top,self.bot*other.bot)
l=input().split()
x=fra(int(l[0]),int(l[1]))
y=fra(int(l[2]),int(l[3]))
print(x+y)
```



代码运行截图 ==（至少包含有"Accepted"）==

![分数类](C:\Users\86133\Desktop\计概作业\数据结构与算法-作业\第二周\分数类.png)

### 04110: 圣诞老人的礼物-Santa Clau’s Gifts

greedy/dp, http://cs101.openjudge.cn/practice/04110



思路：



##### 代码

```python
n,e=map(int,input().split())
l1=[]
for i in range(n):
    v,w=map(int,input().split())
    l1.append([v,w,v/w])
l1.sort(key=lambda x: x[2],reverse=True)
va=0
for j in range(n):
    if e>=l1[j][1]:
        e-=l1[j][1]
        va+=l1[j][0]
    else:
        va+=e*l1[j][2]
        break
print(format(va,".1f"))

```



代码运行截图 ==（至少包含有"Accepted"）==

![圣诞老人](C:\Users\86133\Desktop\计概作业\数据结构与算法-作业\第二周\圣诞老人.png)



### 18182: 打怪兽

implementation/sortings/data structures, http://cs101.openjudge.cn/practice/18182/



思路：



##### 代码

```python
nCases=int(input())
for i in range(nCases):
    n,m,b=map(int,input().split())
    l=[]
    for j in range(n):
        t,x=map(int,input().split())
        l.append((t,x))
    l.sort(key=lambda y:(y[0],-y[1]))
    time=l[0][0]
    ct=0
    for k in range(n):
        if l[k][0]==time:
            ct+=1
        else:
            ct=1
            time=l[k][0]
        if ct<=m:
            b-=l[k][1]
        if b<=0:
            print(l[k][0])
            break
        else:
            if k==n-1:
                print("alive")
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![打怪兽](C:\Users\86133\Desktop\计概作业\数据结构与算法-作业\第二周\打怪兽.png)



### 230B. T-primes

binary search/implementation/math/number theory, 1300, http://codeforces.com/problemset/problem/230/B



思路：



##### 代码

```python
def prime(n):
    s = set()
    ans = 0
    ans = [True] * (n + 1)
    ans[0] = ans[1] = False
    for i in range(2, int(n ** 0.5) + 1):
        if ans[i]:
            for j in range(i * i, n + 1, i):
                ans[j] = False
    for k in range(2, n + 1):
        if ans[k]:
            s.add(k)
    return s


n = int(input())
l = input().split()
l1 = []
for i in range(len(l)):
    l1.append(int(l[i]))
m = max(l1)
ans = prime(int(m ** 0.5))
for i in range(len(l)):
    sq = int(l[i]) ** 0.5
    if sq == sq // 1:
        if int(sq) in ans:
            print("YES")
        else:
            print("NO")
    else:
        print("NO")
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![T--prime](C:\Users\86133\Desktop\计概作业\数据结构与算法-作业\第二周\T--prime.png)



### 1364A. XXXXX

brute force/data structures/number theory/two pointers, 1200, https://codeforces.com/problemset/problem/1364/A



思路：



##### 代码

```python
line=int(input())
for i in range(line):
    n,x=map(int,input().split())
    l=input().split()
    l1=[]
    a=0
    for j in range(len(l)):
        l1.append(int(l[j]))
        if int(l[j])%x==0:
            a+=1
    if a==len(l1):
        print(-1)
    else:
        if sum(l1)%x!=0:
            print(n)
        else:
            for k in range(n):
               if l1[k]%x!=0 or l1[-(k+1)]%x!=0:
                   print(n-k-1)
                   break

              
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240228214202276](C:\Users\86133\AppData\Roaming\Typora\typora-user-images\image-20240228214202276.png)



### 18176: 2050年成绩计算

http://cs101.openjudge.cn/practice/18176/



思路：



##### 代码

```python
s=set()
ans=[True]*10001
ans[0]=ans[1]=False
for i in range(2,101):
    if ans[i]:
        for j in range(i*i,10001,i):
            ans[j]=False
for k in range(2,10001):
    if ans[k]:
        s.add(k)

m,n=map(int,input().split())
for i in range(m):
    grade=[int(_) for _ in input().split()]
    su=0
    for j in range(len(grade)):
        if grade[j]**0.5==int(grade[j]**0.5):
            if grade[j]**0.5 in s:
                su+=grade[j]

    if su==0:
        print(0)
    else:
        print(format(su/len(grade),".2f"))
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![2050成绩计算](C:\Users\86133\Desktop\计概作业\数据结构与算法-作业\第二周\2050成绩计算.png)



## 2. 学习总结和收获

==如果作业题目简单，有否额外练习题目，比如：OJ“2024spring每日选做”、CF、LeetCode、洛谷等网站题目。==

作业较为简单，主要学习了class类的自定义运算，自己进行了用法的尝试和总结，其他题目主要是上个学期内容的回顾



