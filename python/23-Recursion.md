# Recursion

<div align="center"><img src="http://p7erlqn6k.bkt.clouddn.com/image/jpg/python/dg/001-dg.jpg" height="100%" width="40%"/></div>

* __定义：程序调用自身的一种编程技巧__

* 递归问题列表：
    1. $n!$、$a^n$、斐波纳契数列
    2. 枚举、全排列、组合、带条件排列组合、八皇后问题
    3. 树遍历、二叉树搜索、全遍历、求树的深度、二叉搜索树查找、前序后序中序转换
    4. 归并排序、快速排序
    5. 图遍历
    6. 递归实现DP、使用记忆数组降低时间复杂度

* 递归问题相关性质：
    1. 递归的过程相当于对一颗二叉树的遍历（深度优先）过程
    2. 遍历过程形成一颗搜索树
    3. 搜索树的深度影响：O(space)、搜索树叶子节点个数影响：O(time)
    4. 递归的剪枝：利用判定条件减少搜索树中某些分枝的搜索，达到降低算法运行时间复杂度的效果

**1 $n!$、$a^n$、斐波纳契数列**

一个标准的递归形式：求$n!$
```python
def factorial(n):
    if n == 0:
        return 1
    if n == 1:
        return 1
    return n * factorial(n-1)
```

在Fibonacci Series中的发现：
```python
'''
1、1、2、3、5、8、13、21、34、……
F(1)+F(2)=F(3)
F(2)+F(3)=F(4)
...
F(n-2)+F(n-1)=F(n)
'''
def fibonacci(n):
    if n == 1:
        return 1
    if n == 2:
        return 1
    return fibonacci(n-2)+fibonacci(n-1)
```

>程序运行过程中会重复调用参数相同的函数，也就是说程序计算过程有重复，重复的过程会增加时间复杂度。这里采用**记忆数组**的方式，在程序运行过程中，每次记录第一次调用函数的值，下次需要该值时直接从数组中获取。**记忆数组**的技巧多用在DP中。

```python
"""这里采用记忆搜索方式"""
def fib_magicbox(n):
    mb = [-1]*(n+1)
    return fibonacci(n,mb)

def fibonacci(n,mb):
    if mb[n] != -1:
        return mb[n]
    if n == 1:
        mb[1] = 1
        return mb[1]
    if n == 2:
        mb[2] = 1
        return mb[2]
    mb[n] = fibonacci(n-2,mb)+fibonacci(n-1,mb)
    return mb[n]
```

对 $a^n$ 的探索：
```python
def power(a,n):
    if n == 0:
        return 1
    if n == 1:
        return a
    if n % 2 == 0:
        n = n // 2
        return power(a*a,n)
    else:
        n = n - 1
        return a * power(a,n)
```

**2 枚举、全排列、组合、带条件排列组合、八皇后问题**

已知有一个长度为n的数组，数组每一位取值范围为[1,n],求该数组所有可能的结果并返回
>递归、暴力搜索

```python
"""
n = 2
[1,1],[1,2],[2,1],[2,2]
"""
def get_nums(n):
    rst = []
    temp = []
    k = n
    enum(rst,temp,k,n)
    return rst
def enum(rst,temp,k,n):
    if n == 0:
        rst.append(temp.copy())
        return
    for i in range(1,k+1):
        temp.append(i)
        enum(rst,temp,k,n-1)
        temp.pop()
# print(get_nums(3))
# [[1, 1, 1], [1, 1, 2], [1, 1, 3], [1, 2, 1], [1, 2, 2], [1, 2, 3], 
# [1, 3, 1], [1, 3, 2], [1, 3, 3], [2, 1, 1], [2, 1, 2], [2, 1, 3], 
# [2, 2, 1], [2, 2, 2], [2, 2, 3], [2, 3, 1], [2, 3, 2], [2, 3, 3], 
# [3, 1, 1], [3, 1, 2], [3, 1, 3], [3, 2, 1], [3, 2, 2], [3, 2, 3], 
# [3, 3, 1], [3, 3, 2], [3, 3, 3]]
```

现有非负整数[1,n]（n>=1），求这些数的**全排列**
>与上一道题不同的是，同样的数字只能出现一次，该数字前面使用过了，后面就不能使用。

>怎么处理？

>这里使用两种常用的方法，第一种检查临时栈temp中是否存在该元素；第二种是通用的方法，借助辅助数组统计元素是否被使用；

>除此之外，还有两种方法：元素插入法、元素交换法。两种方法都是使用递归，且方法思路精妙，但两种方法不常用，故不在此列出。具体代码在排列组合专题。

```python
'''
方法一：该方法检查临时栈中是否已经存在该元素，思路比较容易理解
'''
def permute(n):
    rst = []
    temp = []
    pos = 0
    helper(rst,temp,pos,n)
    return rst
def helper(rst,temp,pos,n):
    if pos == n:
        rst.append(temp.copy())
        return
    for i in range(1,n+1):
        if i in temp:
            continue
        temp.append(i)
        helper(rst,temp,pos+1,n)
        temp.pop()
'''
方法二：初始化记忆数组，记录该元素是否被使用，总体的思想和方法一相同。
       该方法中的记忆数组在后续问题中会被使用到
'''
def permute(n):
    rst = []
    temp = []
    used = [False]*(n+1)
    pos = 0
    helper(rst,temp,used,pos,n)
    return rst
def helper(rst,temp,used,pos,n):
    if pos == n:
        rst.append(temp.copy())
        return
    for i in range(1,n+1):
        if not used[i]:
            used[i] = True
            temp.append(i)
            helper(rst,temp,used,pos+1,n)
            temp.pop()
            used[i] = False
```

现有元素的集合[1,n]，从中取k个数（0< k <=n），一共有多少种**组合**，即求$C^k_n$

注意：解**“组合类”**问题时，遍历的过程是**“不回头”**的

```python
def combine(n,k):
    rst = []
    temp = []
    num = 1
    helper(rst,temp,num,k,n)
    return rst
def helper(rst,temp,num,k,n):
    if len(temp) == k:
        rst.append(temp.copy())
        return
    for i in range(num,n+1):
        temp.append(i)
        helper(rst,temp,i+1,k,n) # 这里的i体现“递归”过程中“不回头”的思想
        temp.pop()
# rst = combine(4,2)
# [[1, 2], [1, 3], [1, 4], [2, 3], [2, 4], [3, 4]]
```

集合A由非负、不重复的整数组成，从集合A中取若干数，使得若干数之和为常数t
>nums=[1,3,6,7]  t=7

```python
"""
"""
def screen(nums,t):
    rst = []
    temp = []
    pos = 0
    helper(rst,temp,pos,t,nums)
    return rst
def helper(rst,temp,pos,t,nums):
    if sum(temp) >= t:
        if sum(temp) == t:
            rst.append(temp.copy())
        return
    for i in range(pos,len(nums)):
        temp.append(nums[i])
        helper(rst,temp,i+1,t,nums)
        temp.pop()

# arr = [1,3,6,7]
# t = 7
# rst:[[1, 6], [7]]
```

求数组nums的**子集**
>nums = [1,2,3] rst = [[], [1], [1, 2], [1, 2, 3], [1, 3], [2], [2, 3], [3]]

```python
"""

"""
def subset(nums):
    rst = [[]]
    temp = []
    pos = 0
    helper(rst,temp,pos,nums)
    return rst
def helper(rst,temp,pos,nums):
    if pos == len(nums):
        return
    for i in range(pos,len(nums)):
        temp.append(nums[i])
        rst.append(temp.copy())
        helper(rst,temp,i+1,nums)
        temp.pop()
```

**可以看到，子集问题同上面的求和为常数问题以及上面的组合问题，其核心递归函数调用部分的思路是一致的**

下面的问题是排列组合子集类问题的一个扩展，求解**八皇后**。

一个二维棋盘由8行8列组成，现有8个皇后要摆放在棋盘上，要求8个皇后在棋盘上的位置随意，但是同一行、同一列、同一左、右对角线上不能出现第二个皇后。问一共有多少种摆法，并列出所有的摆法。
>暴力枚举、条件限制

>怎样处理“同一行、同一列、左右对角线”问题？问题抽象，同一对角线上元素x、y下标经过组合为定值。借助记忆数组，来标志同一行、左右对角线是否可以摆放元素。为什么同一列不需要限制？因为算法从列开始遍历。

>与排列问题相似，借助记忆数组添加限制条件，只是这里的限制条件增加了

通过对比下面的两种方法，可以看出方法一更符合递归含义，并且时间复杂度远小于第二种。推荐使用第一种方法。第二种方法的思路有局限性，不适合解决八皇后问题。

```python
"""
该方法利用递归调用函数的方式控制x变量（外层），利用内层for循环控制y变量
推荐使用的方法
"""
def eight_queen(n):
    rst = []
    temp = []
    used_y = [False] * (n)
    used_x_y = [False] * (n*2)
    used_x_y_n = [False] * (n*2)
    helper(rst,temp,used_y,used_x_y,used_x_y_n,n-1,n)
    return rst
# 递归控制x，每一层中固定的foreach控制y
def helper(rst,temp,used_y,used_x_y,used_x_y_n,x,n):
    if x == -1:
        rst.append(copy.deepcopy(temp))
        return
    for y in range(n): # 这里的y在每一递归层都是固定的扫描范围
        if not used_y[y] and not used_x_y[x + y] and not used_x_y_n[x - y + n]:
            used_y[y] = True
            used_x_y[x + y] = True
            used_x_y_n[x - y + n] = True
            temp.append([x,y].copy())
            helper(rst,temp,used_y,used_x_y,used_x_y_n,x-1,n) # 递归的时候控制x轴发生变化：-1
            temp.pop()
            used_y[y] = False
            used_x_y[x+y] = False
            used_x_y_n[x-y+n] = False


"""
该方法不推荐
说明该种思路在解决问题时的局限性
"""
def eight_queen(n):
    rst = []
    temp = []
    pos = 0
    used_y = [False]*(n)
    used_x_y = [False]*(n*2)
    used_x_y_n = [False]*(n*2)
    helper(rst,temp,pos,n,used_y,used_x_y,used_x_y_n)
    return rst
def helper(rst,temp,pos,n,used_y,used_x_y,used_x_y_n):
    if len(temp) == n:
        rst.append(copy.deepcopy(temp))
        return
    for x in range(pos,n):
        for y in range(n):
            if not used_y[y] and not used_x_y[x+y] and not used_x_y_n[x-y+n]:
                used_y[y] = True
                used_x_y[x+y] = True
                used_x_y_n[x-y+n] = True
                temp.append([x,y].copy())
                helper(rst,temp,x+1,n,used_y,used_x_y,used_x_y_n)
                temp.pop()
                used_y[y] = False
                used_x_y[x+y] = False
                used_x_y_n[x-y+n] = False
```

使用递归常遇到的一个问题就是因为递归深度过深引起的爆栈，内存溢出。

**3 树遍历、二叉树搜索、全遍历、求树的深度、二叉搜索树查找、前序后序中序转换**

根据树的preorder、inorder或者postorder、inorder构建二叉树
```
preorder = [3,9,20,15,7]
inorder = [9,3,15,20,7]
postorder = [9,15,7,20,3]
```

<div align="center"><img src="http://p7erlqn6k.bkt.clouddn.com/image/jpg/python/tree/013-tree.png" height="100%" width="20%"/></div>

```python
"""preorder+inorder"""
class PreInRecoverBinaryTree:
    def buildTree(self,preorder,inorder):
        if preorder:
            pre_start,pre_end,in_start,in_end = 0,len(preorder),0,len(inorder)
            return self.helper(preorder,inorder,pre_start,pre_end,in_start,in_end)
        return None
    def helper(self,preorder,inorder,pre_start,pre_end,in_start,in_end):
        if pre_start == pre_end:
            return None
        # 在inorder中寻找root
        cur = in_start
        while cur < in_end:
            if inorder[cur] == preorder[pre_start]:
                break
            cur += 1
        root = TreeNode(inorder[cur])
        root.left = self.helper(preorder,inorder,pre_start+1,pre_start+cur-in_start+1,in_start,cur)
        root.right = self.helper(preorder,inorder,pre_start+cur-in_start+1,pre_end,cur+1,in_end)
        return root
```

递归问题还有很多，能用递归思路解决的问题都可以归纳为递归问题。树型左右子树结构天生可以利用递归来解决，更过的解法有待补充和在其他专题中呈现递归。

后面的部分是递归解法题目的记录：
---

* Longest Substring with At Least K Repeating Characters

>Find the length of the longest substring T of a given string（consists of lowercase letters only）such that every character in T appears no less than k times.

>example:

```
input:
s = 'aaabb',k = 3
output:
3
The longest substring is 'aaa',as 'a' is repeated 3 times.

input:
s = 'ababbc',k = 2
output:
5
The longest substring is 'ababb',as 'a' is repeated 2 times and 'b' is repeated 3 times.
```

题目要求子字符串中每个元素重复出现次数大于等于k个的最长子字符串的长度。乍一看题目比较复杂，要求符合条件的最长子字符串长度，怎样寻找符合元素重复个数大于等于k的子字符串？似乎可以用DP，因为遇到所谓“最长”。其实，此题不一定要用DP，或者说本题不适合DP算法来解。

我们观察，如果一个字符在字符串中出现的次数`count(c)<k`，那么该字符必定不会出现在符合条件的子字符串中，我们怎么对待这些出现次数小于k的字符呢？直接将这些字符替换掉，以这些字符为分割线，将原来的字符串分割为多个不包含这些不符合条件字符的子字符串，我们想要的结果就在这些分割后的子字符串中，这里用到了**分治**的思想，分别在这些子字符串中寻找答案。因为这样的过程要不断的重复处理，我们使用递归来实现。递归的终止条件是什么？当字符串长度小于k时，就没有必要在处理了；或者字符串本身是一个满足条件的字符串，直接返回该字符串长度。我们要做的就是取最大值。

```python
class Solution:
    def longestSubstring(self,s,k):
        rst = 0
        if len(s) < k:
            return 0
        cakes = sorted(Counter(s).most_common(),key= lambda item:item[1])
        for cake in cakes:
            if cake[1]<k:
                s = s.replace(cake[0],'-')
        if '-' not in s:
            return len(s)
        substrs = list(set(s.split('-')))
        for substr in substrs:
            rst = max(rst,self.longestSubstring(substr,k))
        return rst
```

















