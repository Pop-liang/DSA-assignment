# Assignment #3: March月考

Updated 1537 GMT+8 March 6, 2024

2024 spring, Complied by ==施广熠 生命科学学院==



**说明：**

1）The complete process to learn DSA from scratch can be broken into 4 parts:
- Learn about Time and Space complexities
- Learn the basics of individual Data Structures
- Learn the basics of Algorithms
- Practice Problems on DSA

2）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

4）如果不能在截止前提交作业，请写明原因。



**编程环境**

==（请改为同学的操作系统、编程环境等）==

操作系统：windous11

Python编程环境：PyCharm 2023.1.4 (Professional Edition)

C/C++编程环境：无



## 1. 题目

**02945: 拦截导弹**

http://cs101.openjudge.cn/practice/02945/



思路：



##### 代码

```python
n=int(input())
height=[int(i) for i in input().split()]
dp_r=[1]*n
for i in range(n-2,-1,-1):
    for j in range(n-1,i,-1):
        if height[i]>=height[j]:
            dp_r[i]=max(dp_r[i],dp_r[j]+1)
print(max(dp_r))
```



代码运行截图 ==（至少包含有"Accepted"）==

![拦截导弹](C:\Users\86133\Desktop\计概作业\数据结构与算法-作业\拦截导弹.png)



**04147:汉诺塔问题(Tower of Hanoi)**

http://cs101.openjudge.cn/practice/04147



思路：



##### 代码

```python
l=input().split()
n=int(l[0])
a=l[1]
b=l[2]
c=l[3]
def move(n,start,other,end):
    if n==1:
        print("1:"+start+"->"+end)
    else:
        move(n-1,start,end,other)
        print(str(n)+":"+start+"->"+end)
        move(n-1,other,start,end)
move(n,a,b,c)

```



代码运行截图 ==（至少包含有"Accepted"）==

![汉诺塔](C:\Users\86133\Desktop\计概作业\数据结构与算法-作业\汉诺塔.png)



**03253: 约瑟夫问题No.2**

http://cs101.openjudge.cn/practice/03253



思路：



##### 代码

```python
while True:
    n,p,m=map(int,input().split())
    if n==0:
        exit()
    else:
        l=[True]*n
        pin=p-1
        time=0
        c=0
        while True:
            if l[pin]==True:
                time+=1
                if time==m:
                    time=0
                    l[pin]=False
                    c+=1
                    if c==n:
                        print(pin+1)
                        break
                    else:
                        print(pin+1,end=",")
            if pin==n-1:
                pin=0
            else:
                pin+=1

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![约瑟夫2](C:\Users\86133\Desktop\计概作业\数据结构与算法-作业\约瑟夫2.png)



**21554:排队做实验 (greedy)v0.2**

http://cs101.openjudge.cn/practice/21554



思路：



##### 代码

```python
n=int(input())
list=[int(i) for i in input().split()]
l=[(list[j],j+1) for j in range(n)]
l.sort()
for k in range(n):
    print(l[k][1],end=" ")
su=0
for r in range(n-1):
    su+=l[r][0]*(n-1-r)
print()
print(format(su/n,".2f"))
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![排队实验](C:\Users\86133\Desktop\计概作业\数据结构与算法-作业\排队实验.png)



**19963:买学区房**

http://cs101.openjudge.cn/practice/19963



思路：



##### 代码

```python
n=int(input())
pairs = [i[1:-1] for i in input().split()]
l= [ sum(map(int,i.split(','))) for i in pairs]
price=[int(i) for i in input().split()]
d_v=[0]*n
for k in range(n):
    d_v[k]=l[k]/price[k]
store1=[i for i in price]
store2=[j for j in d_v]
d_v.sort()
price.sort()
if n%2==0:
    mid_v=(price[n//2-1]+price[n//2])/2
    mid_r=(d_v[n//2-1]+d_v[n//2])/2
else:
    mid_v=price[(n+1)//2-1]
    mid_r=d_v[(n+1)//2-1]
su=0
for j in range(n):
    if store1[j]<mid_v and store2[j]>mid_r:
        su+=1
print((su))
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![学区买房](C:\Users\86133\Desktop\计概作业\数据结构与算法-作业\学区买房.png)



**27300: 模型整理**

http://cs101.openjudge.cn/practice/27300



思路：



##### 代码

```python
dic={}
n=int(input())
for i in range(n):
    inf=input().split("-")
    try:
        dic[inf[0]]=dic[inf[0]]+[inf[1]]
    except KeyError:
        dic[inf[0]]=[inf[1]]
name=[j for j in dic.keys()]  #不能直接sort，只能遍历其中元素
name.sort()
def ssort(l):
    B=[]
    M=[]
    for i in range(len(l)):
        if l[i][-1]=="B":
            B.append(float(l[i][0:-1]))
        else:
            M.append(float(l[i][0:-1]))
    B.sort()
    for j in range(len(B)):
        if int(B[j])==B[j]:
            B[j]=str(int(B[j]))+"B"
        else:
            B[j]=str(B[j])+"B"
    M.sort()
    for k in range(len(M)):
        if int(M[k])==M[k]:
            M[k]=str(int(M[k]))+"M"
        else:
            M[k]=str(M[k])+"M"
    return M+B

for i in name:
    ans=ssort(dic[i])
    print(i+": "+", ".join(j for j in ans))
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![模型整理](C:\Users\86133\Desktop\计概作业\数据结构与算法-作业\模型整理.png)



## 2. 学习总结和收获

==如果作业题目简单，有否额外练习题目，比如：OJ“2024spring每日选做”、CF、LeetCode、洛谷等网站题目。==

作业题目涉及dp，字符串，greedy等比较典型的问题，约瑟夫问题2中使用了更好的方式解决，模型整理一题处理和输出都较为繁琐，感觉意义不大，可能还有较为简单高效的方法



