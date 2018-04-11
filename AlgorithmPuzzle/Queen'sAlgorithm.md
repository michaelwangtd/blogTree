# Queen's Algorithm
![www.baidu.com](http://p6utxby50.bkt.clouddn.com/queen_algorithm_start.jpg)
>算法的学习是漫长而有趣的，就像吉他、萨克斯之于音乐，慢跑之于锻炼，坚持最重要。希望在日常生活中学习算法、积累算法，达到算法之于编程的境界。

>（以较高的水准、丰富的版式、易懂的文字记录下经典有趣的算法）

* 题目：__Subset__
* 类别：__递归+回溯+DFS、子集、集合__

描述：给定不相同整数的集合nums，返回所有可能的子集。返回集合不能包含重复的子集。例如：nums= [ 1,2,3 ]，它的一个解决方案是：[ [], [1], [1, 2], [1, 2, 3], [1, 3], [2], [2, 3], [3] ]
>背景：总体上有三种方法解决该问题。</br>
>位运算法：是比较巧妙的一种方法，将数组nums索引与二进制位对应起来，每一位都有‘1’或‘0’的两种可能，该方法速度最慢。</br>
组合数扩展法：从空列表[]开始，依次遍历nums中元素添加进结果集，使用两个for循环就能实现，该方法速度最快。</br>
递归+回溯+DFS：方法很巧妙，借助递归函数中套用for循环形式，在维护的临时列表temp中“一进一出”增加、删除元素，达到深度优先遍历，该方法速度第二。

解法一：位运算法
```python
def subset1(self,nums):
    # nums = sorted(nums)
    rst = []
    end = 1<<len(nums)
    for bit in range(end): # 用遍历十进制方式表示二进制位操作
        temp = []
        for i in range(len(nums)-1,-1,-1): # 对每一位进行操作
            if bit & 1<<i:
                temp.append(nums[len(nums)-1-i])
        rst.append(temp)
    return rst
```
解法二：组合数扩展法
```python
def subset2(self,nums):
    cakes = [[]]
    for num in nums:
        single = []
        for cake in cakes:
            temp = cake.copy()
            temp.append(num)
            single.append(temp)
        cakes.extend(single)
    return cakes
```
解法三：递归+回溯+DFS
```python
def subset3(self,nums):
    rst = [[]]
    temp = []
    pos = 0
    self.subset_helper(rst,temp,pos,nums)
    return rst
def subset_helper(self,rst,temp,pos,nums):
    if pos == len(nums):
        return
    for i in range(pos,len(nums)):
        temp.append(nums[i])
        print('subset:',temp,'pos:',pos,'i:',i)
        rst.append(temp.copy())
        self.subset_helper(rst,temp,i+1,nums)
        temp.pop()
```