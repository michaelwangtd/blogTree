# Permutation and Combination（排列组合子集类问题）

<div align="center"><img src="http://p7erlqn6k.bkt.clouddn.com/image/jpg/python/dp/001-dp.jpg" height="100%" width="80%"/></div>

>排列组合子集类问题有相似的共性，该类问题及其变形问题有相似的处理方式，俗称“套路”。

我们先来看看如何获得一个数组的所有的划分。要找到所有的划分，先要清楚什么是数组的一个划分。“划分”这个词让人联想到决策树中寻找最优划分属性，这里的划分类似，在不改变数组顺序情况下，把数组分成长度更小的几个子数组，这些子数组不重复、不遗漏的组成原始数组，这就是一个划分。

如何获得一个划分，以至获得所有划分。遍历什么？搜索树？因为划分中的子数组等同于子集，所以将问题划归为排列组合子集类问题。

排列组合子集类问题是对一颗搜索树进行深度优先遍历的过程，遍历过程中节点的记录要用到回溯法，而这一过程由于树结构的特性，采用递归的方式实现。

我们看看采用经典的递归+DFS+回溯方式能得到什么结果。
```python
class DFS:
    def dfs(self,nums):
        pos = 0
        cache = []
        rst = []
        self.helper(nums,pos,cache,rst)
        return rst
    def helper(self,nums,pos,cache,rst):
        if pos == len(nums):
            rst.append(cache.copy())
            return 
        for i in range(pos,len(nums)):
            cache.append([pos,i])
            self.helper(nums,i+1,cache,rst)
            cache.pop()
```

我们传入测试数组`nums = [1,2,3,4]`，将结果转换为对应的子数组，并输出，我们得到了数组的所有的划分：

```
# 指针
[[0, 0], [1, 1], [2, 2], [3, 3]]
[[0, 0], [1, 1], [2, 3]]
[[0, 0], [1, 2], [3, 3]]
[[0, 0], [1, 3]]
[[0, 1], [2, 2], [3, 3]]
[[0, 1], [2, 3]]
[[0, 2], [3, 3]]
[[0, 3]]
# 划分集合
[[1], [2], [3], [4]]
[[1], [2], [3, 4]]
[[1], [2, 3], [4]]
[[1], [2, 3, 4]]
[[1, 2], [3], [4]]
[[1, 2], [3, 4]]
[[1, 2, 3], [4]]
[[1, 2, 3, 4]]
```

可以看到数组的所有划分中包含了数组所有的子数组。我们明确一些概念：

>子集、子序列：子集属于集合的概念，通常我们说求一个集合的子集。子序列对应于数组，求一个数列、数组中的子序列。这两个概念有一个共同特点，子集、子序列对应原集合、原序列可以是不相邻的，比如：1,2,3。它的子集有1,3，它的子序列也有1,3。

>子数组：子数组必须保持原数组相对顺序，并且元素是相邻的。

* Palindrome Partitioning

>Given a string s,partition s such that every substring of the partition is a palindrome.Return all possible palindrome partitioning of s.

>For example,given s='aab',Return
```
[
    ['aa','b']
    ['a','a','b']
]
```

题目要求子数组都是回文的划分的集合，我们可以拆分需求，要求出数组所有的划分，并判断划分中的子数组是否都是回文。判断回文我们用双指针封装函数，那么如何求数组的划分呢？借助上面的“套路”求划分，并在过程中判断回文。

```python
class PalindromePartition:
    def palindrome_partition(self,s):
        pos = 0
        cache = []
        rst = []
        self.helper(s,pos,cache,rst)
        return rst
    def helper(self,s,pos,cache,rst):
        if pos == len(s):
            rst.append(cache.copy())
            return
        for i in range(pos,len(s)):
            if self.is_palindrome(s,pos,i):
                cache.append(s[pos:i+1])
                self.helper(s,i+1,cache,rst)
                cache.pop()
    def is_palindrome(self,s,i,j):
        while i <= j:
            if s[i] != s[j]:
                return False
            i += 1
            j -= 1
        return True
```

```
input：
    s = 'aabb'
output:
    ['a', 'a', 'b', 'b']
    ['a', 'a', 'bb']
    ['aa', 'b', 'b']
    ['aa', 'bb']
```