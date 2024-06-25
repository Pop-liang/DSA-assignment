# Assignment #5: "树"算：概念、表示、解析、遍历

Updated 2124 GMT+8 March 17, 2024

2024 spring, Complied by ==施广熠 生命科学学院==



**说明：**

1）The complete process to learn DSA from scratch can be broken into 4 parts:

Learn about Time complexities, learn the basics of individual Data Structures, learn the basics of Algorithms, and practice Problems.

2）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

4）如果不能在截止前提交作业，请写明原因。



**编程环境**

==（请改为同学的操作系统、编程环境等）==

操作系统：windous11

Python编程环境： PyCharm 2023.1.4 (Professional Edition)

C/C++编程环境：无

## 1. 题目

### 27638: 求二叉树的高度和叶子数目

http://cs101.openjudge.cn/practice/27638/



思路：



代码

```python
class TreeNode:
    def __init__(self):
        self.left = None
        self.right = None

def tree_height(node):
    if node is None:
        return -1
    return max(tree_height(node.left), tree_height(node.right)) + 1

def count_leaves(node):
    if node is None:
        return 0
    if node.left is None and node.right is None:
        return 1
    return count_leaves(node.left) + count_leaves(node.right)

n = int(input())
nodes = [TreeNode() for _ in range(n)]
has_parent = [False] * n

for i in range(n):
    left_index, right_index = map(int, input().split())
    if left_index != -1:
        nodes[i].left = nodes[left_index]
        has_parent[left_index] = True
    if right_index != -1:
        #print(right_index)
        nodes[i].right = nodes[right_index]
        has_parent[right_index] = True

root_index = has_parent.index(False)
root = nodes[root_index]

height = tree_height(root)
leaves = count_leaves(root)

print(height,leaves)
```



代码运行截图 ==（至少包含有"Accepted"）==

![叶子](C:\Users\86133\Desktop\计概作业\数据结构与算法-作业\叶子.png)



### 24729: 括号嵌套树

http://cs101.openjudge.cn/practice/24729/



思路：



代码

```python
class node:
    def __init__(self,val):
        self.value=val
        self.children=[]

def preorderTraversal(root):
    # 递归法
    if not root: return []
    result = []

    def traversal(root):
        if not root:
            return
        result.append(root.val)  # 先将根节点值加入结果
        if root.left:
            traversal(root.left)  # 左
        if root.right:
            traversal(root.right)  # 右

    traversal(root)
    return result
def postorderTraversal(self, root: Optional[node]) -> list[int]:
    # 递归遍历
    if not root: return []
    result = []

    def traversal2(root):
        if not root:
            return
        traversal2(root.left)  # 左
        traversal2(root.right)  # 右
        result.append(root.val)  # 中
    traversal2(root)
    return result

def parse_tree(s):
    stack = []
    node = None
    for char in s:
        if char.isalpha():  # 如果是字母，创建新节点
            node = node(char)
            if stack:  # 如果栈不为空，把节点作为子节点加入到栈顶节点的子节点列表中
                stack[-1].children.append(node)
        elif char == '(':  # 遇到左括号，当前节点可能会有子节点
            if node:
                stack.append(node)  # 把当前节点推入栈中
                node = None
        elif char == ')':  # 遇到右括号，子节点列表结束
            if stack:
                node = stack.pop()  # 弹出当前节点
    return node  # 根节点
s=input()
root=parse_tree(s)
print(preorderTraversal(root))
print(postorderTraversal(root))
```



代码运行截图 ==（至少包含有"Accepted"）==

![括号嵌套树](C:\Users\86133\Desktop\计概作业\数据结构与算法-作业\括号嵌套树.png)



### 02775: 文件结构“图”

http://cs101.openjudge.cn/practice/02775/



思路：



代码

```python
def print_structure(node,indent=0):

    prefix='|     '*indent
    print(prefix+node['name'])
    for dir in node['dirs']:
        
        print_structure(dir,indent+1)
    for file in sorted(node['files']):

        print(prefix+file)
dataset=1
datas=[]
temp=[]


while True:
    line=input()
    if line=='#':
        break
    if line=='*':
        datas.append(temp)
        temp=[]
    else:
        temp.append(line)
for data in datas:
    print(f'DATA SET {dataset}:')
    root={'name':'ROOT','dirs':[],'files':[]}
    stack=[root]

    
    for line in data:
        if line[0]=='d':
            dir={'name':line,'dirs':[],'files':[]}
            stack[-1]['dirs'].append(dir)
            stack.append(dir)
        elif line[0]=='f':
            stack[-1]['files'].append(line)
        else:  
            stack.pop()
    print_structure(root)
    if dataset<len(datas):
        print()
    dataset+=1
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![文件图结构](C:\Users\86133\Desktop\计概作业\数据结构与算法-作业\文件图结构.png)



### 25140: 根据后序表达式建立队列表达式

http://cs101.openjudge.cn/practice/25140/



思路：



代码

```python
class TreeNode:
    def __init__(self, value):
        self.value = value
        self.left = None
        self.right = None

def build_tree(postfix):
    stack = []
    for char in postfix:
        node = TreeNode(char)
        if char.isupper():
            node.right = stack.pop()
            node.left = stack.pop()
        stack.append(node)
    return stack[0]

def level_order_traversal(root):
    queue = [root]
    traversal = []
    while queue:
        node = queue.pop(0)
        traversal.append(node.value)
        if node.left:
            queue.append(node.left)
        if node.right:
            queue.append(node.right)
    return traversal

n = int(input().strip())
for _ in range(n):
    postfix = input().strip()
    root = build_tree(postfix)
    queue_expression = level_order_traversal(root)[::-1]
    print(''.join(queue_expression))
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![根据后序](C:\Users\86133\Desktop\计概作业\数据结构与算法-作业\根据后序.png)



### 24750: 根据二叉树中后序序列建树

http://cs101.openjudge.cn/practice/24750/



思路：



代码

```python
def build_tree(inorder, postorder):
    if not inorder or not postorder:
        return []

    root_val = postorder[-1]
    root_index = inorder.index(root_val)

    left_inorder = inorder[:root_index]
    right_inorder = inorder[root_index + 1:]

    left_postorder = postorder[:len(left_inorder)]
    right_postorder = postorder[len(left_inorder):-1]

    root = [root_val]
    root.extend(build_tree(left_inorder, left_postorder))
    root.extend(build_tree(right_inorder, right_postorder))

    return root


def main():
    inorder = input().strip()
    postorder = input().strip()
    preorder = build_tree(inorder, postorder)
    print(''.join(preorder))


if __name__ == "__main__":
    main()
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![根据二叉树中后序](C:\Users\86133\Desktop\计概作业\数据结构与算法-作业\根据二叉树中后序.png)



### 22158: 根据二叉树前中序序列建树

http://cs101.openjudge.cn/practice/22158/



思路：



代码

```python
class TreeNode:
    def __init__(self, value):
        self.value = value
        self.left = None
        self.right = None

def build_tree(preorder, inorder):
    if not preorder or not inorder:
        return None
    root_value = preorder[0]
    root = TreeNode(root_value)
    root_index_inorder = inorder.index(root_value)
    root.left = build_tree(preorder[1:1+root_index_inorder], inorder[:root_index_inorder])
    root.right = build_tree(preorder[1+root_index_inorder:], inorder[root_index_inorder+1:])
    return root

def postorder_traversal(root):
    if root is None:
        return ''
    return postorder_traversal(root.left) + postorder_traversal(root.right) + root.value

while True:
    try:
        preorder = input().strip()
        inorder = input().strip()
        root = build_tree(preorder, inorder)
        print(postorder_traversal(root))
    except EOFError:
        break
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![根据二叉树前中序](C:\Users\86133\Desktop\计概作业\数据结构与算法-作业\根据二叉树前中序.png)



## 2. 学习总结和收获

==如果作业题目简单，有否额外练习题目，比如：OJ“2024spring每日选做”、CF、LeetCode、洛谷等网站题目。==

这次主要就是对前中后序表达和树的解析进行进一步的深化，树这种结构在程序中细节很多，容易编译错误，在自己调试程序时也经常遇到奇怪的错误，需要加强练习



