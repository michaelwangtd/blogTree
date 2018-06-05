# DP（Dynamic Programing）
<div align="center"><img src="http://p7erlqn6k.bkt.clouddn.com/image/jpg/python/dp/001-dp.jpg" height="100%" width="80%"/></div>

* leetcode dp题目总结：[https://blog.csdn.net/king_like_coding/article/details/52904122](https://blog.csdn.net/king_like_coding/article/details/52904122)
* leetcode 所有题目分类：[https://www.douban.com/note/330562764/](https://www.douban.com/note/330562764/)

> 作为比较重要偏实践的数据结构，本节打算花费一定力气总结一下动态规划的问题

```
1. 动态规划精髓：加缓存 cache
2. 动态规划本质：递归+缓存
3. 分析问题，确定状态的时候要记得画流程状态树，弄清楚怎么递归，递归是否重复
4. 总体策略：先暴力搜索，再找冗余，再去掉冗余
```

```
最优子结构：
    1）子问题最优决策可导出原问题最优决策：N-1 ----> N
    2）无后效性
重叠子问题：
    1）去冗余
    2）空间换时间：加缓存
套路：最优、最大、最小、最长、计数
离散问题：一般是离散问题
```



* 从 House Robber 问题开始
> 你是一个专业的强盗，打算在街上抢劫房屋。每间房子都藏着一定数量的钱，唯一能组织你抢劫的是相邻的房子都有保安系统连接，如果相邻的两栋房子在同一晚上被闯入，它就会自动联系警察。现有一个代表每所房子的金额的非负整数清单，确定你今晚可以抢劫的最高金额，而无需报警。

> （以上翻译自腾讯翻译君，真TM强大，我是拍照直翻，谷歌、百度都弱爆了，这叫北外的学生们怎么活）


```python
"""
分析：有一非负数组，从中取数，不能相连取，取到的值的和怎样最大？
      换一种问法，递归式思考，在这n个数中怎么取值，最后和最大？
      递归式思考：
                如果取第n个数，那么第n-1个就不能取，
                如果不取第n个数，那么可以取第n-1个数
      f(n):代表在n个数中取值，满足条件下，取到的最大值
      递归式：f(n) = max( arr[n] + f(n-2) , f(n-1) )
      递归终止条件：n>0
              或者：if n<=0:return 0
      根据分析，尝试写出递归+暴力搜索的版本
"""
"""
    一：递归 + 暴力搜索
"""
def house_robber(nums):
    n = len(nums)
    return helper(n-1,nums)
def helper(n,nums):
    if n < 0:
        return 0
    return max(nums[n] + helper(n-2,nums),helper(n-1,nums))
```

```python
"""
分析：上面的代码思路没有问题，逻辑是正确的，但是时间超出了范围
    为什么会超时？找冗余？哪里有冗余？
    我们分析发现，在递归过程中相同的参数i，会重复调用函数f(i)
    由于递归的树形遍历方式，左子树某处调用f(i)之后，在右子树的某处又要调用f(i)
    重复调用就是重复计算，出现重叠子问题重复计算的冗余
    找到冗余，我们需要做的是去掉冗余，怎么去掉冗余？
    加缓存，使用map(k,v)，计算过的值直接返回结果
"""
"""
    二：找冗余，去冗余
"""
def house_robber(nums):
    n = len(nums)
    dic = dict()
    return helper(n-1,nums,dic)
def helper(n,nums,cache):
    if n < 0:
        return 0
    if n in cache.keys():
        return cache[n]
    cache[n] = max(nums[n] + helper(n-2,nums,cache),helper(n-1,nums,cache))
    return cache[n]
```

单独拿出上面两段代码核心步骤：我们发现有相同的递归表达式
```python
"""
    三：分析递归表达式
"""
max(nums[n] + helper(n-2,nums),helper(n-1,nums))

max(nums[n] + helper(n-2,nums,cache),helper(n-1,nums,cache))
```

```python
"""
分析：递归的方式来编写代码，使代码逻辑清晰，易于表达。
    但同时，递归深度较深时容易爆栈，递归代码不透明。
    所以将递归改为递推，怎么改为递推？
    递推肯定是自底向上的，怎么从“底”开始？
    子问题的最优解就是原问题的最优解
    “底”是从子问题的边界条件处开始，递推的推到原问题
    借助线性数组为cache相比字典为cache，要节省空间，故cache也改为数组实现
    边界条件的“底”怎么确定？
    有特殊值的要初始化特殊值；边界条件一般从0开始，可以能有其他
    “自底向上”的过程类似“递归改递推”的过程：
    从0开始foreach数组，同时把最开始简单的几种情况就开始初始化好，后面的遍历直接用
    注意简单条件的值要有直接返回的逻辑

    之前有人说从递归到递推过程，递归表达式不用修改，直接无脑的使用。为啥可以这样说？
    因为，满足最优子结构性质。子问题的最优解就是原问题的最优解，所以也解释了递归和递推的表达式相同。
"""
"""
   四：自底向上，改递归为递推 
"""
def house_robber_traverse(nums):
    if nums:
        n = len(nums)
        cache = [-1] * n
        if n == 1:
            return nums[0]
        if n == 2:
            return max(nums[0], nums[1])
        cache[0], cache[1] = nums[0], max(nums[0], nums[1])
        for i in range(2, n):
            cache[i] = max(nums[i] + cache[i - 2], cache[i - 1])
        return cache[i]
    return 0
```

```python
"""
分析：递推式代码结构清楚，空间开销较递归式减少
    同时我们发现，这个cache其实并不是每次都会用到每一个值
    在遍历过程中cache[i]只与cache[i-1],cache[i-2]有关
    所以我们可以压缩数组cache为三个变量cache1,cache2,cache3
    循环给三个变量赋值，这样就能进一步节省空间
    我们尝试进行循环数组的优化
"""
"""
   五：循环数组 
"""
def house_robber_traverse(nums):
    if nums:
        n = len(nums)
        cache1,cache2,cache3 = 0,0,0
        if n == 1:
            return nums[0]
        if n == 2:
            return max(nums[0], nums[1])
        cache1, cache2 = nums[0], max(nums[0], nums[1])
        for i in range(2, n):
            cache3 = max(nums[i] + cache1, cache2)
            cache1 = cache2
            cache2 = cache3
        return cache3
    return 0
```


```
通过以上分析，总结：
解决动态规划问题的基本步骤：
    一：递归+暴力搜索（明确转化题目含义到递归解决）
    二：找出冗余，使用cache（dict）解决缓存
    三：分析问题的递归表达式
    四：自底向上，改递归为递推
    五：循环数组
```

下面的内容基本围绕熟悉解决问题步骤，直接使用第五步解题展开

* 小兵向前冲
> 现有一N行M列的棋盘(N,M)，一小兵从棋盘左下角走到右上角，规定小兵只能向上向右走，且每次只能走一步，请问一共有多少种走法？

有网友从理论上解答了同样问题，但是从左上走到右下的走法，解答过程也是代码执行过程，很清楚。
<div align="center"><img src="http://p7erlqn6k.bkt.clouddn.com/image/jpg/python/dp/002-dp.png" height="100%" width="40%"/></div>

```
原问题子问题分析，子问题最优解等同于原问题最优解：
f(m,n)：当棋盘大小为(m,n)时一共有f(m,n)种走法

f(m,n) = f(m-1,n) + f(m,n-1)    m*n>0
1                               other
（为什么是m*n>0？matrix矩阵下标从0开始）
```

```python
"""
    递归+暴力搜索
"""
def soldier_walk(m,n):
    m,n = m-1,n-1
    if m>=0 and n>=0:
        return helper(m,n)
    return 0
def helper(m,n):
    if m*n == 0:
        return 1
    return helper(m-1,n) + helper(m,n-1)

"""
    找冗余，去冗余
"""
def soldier_walk(p,q):
    m,n = p-1,q-1
    if m>=0 and n>=0:
        cache = [[-1]*q for i in range(p)]
        return helper(m,n,cache)
    return 0
def helper(m,n,cache):
    if cache[m][n] > -1:
        return cache[m][n]
    if m*n == 0:
        cache[m][n] = 1
        return cache[m][n]

    cache[m][n] = helper(m-1,n,cache) + helper(m,n-1,cache)
    return cache[m][n]

"""
    自底向上，递推实现
"""
def soldier_walk(m,n):
    if m>0 and n>0:
        # 初始化m*n数组
        cache = [[-1]*n for i in range(m)]
        # 处理m*n=0的情况
        for j in range(n):
            cache[0][j] = 1
        for i in range(m):
            cache[i][0] = 1
        if m==1 or n==1:
            return cache[m-1][n-1]
        for i in range(1,m):
            for j in range(1,n):
                cache[i][j] = cache[i-1][j] + cache[i][j-1]
        return cache[i][j]
    return 0
```

* 01背包问题

> 在n个物品中挑选若干物品装入背包，最多背包能装多少？背包限制容量为m，每个物品i的大小为A[i]。

>example：A=[2,3,5,7] m=11 return:10

>程序输入物品数组A，背包容量m，返回背包能装最大的容量数 

读完题发现本题和之前的HouseRobber问题比较像，唯一不同的地方是本题有一个容量m的限制，最多只能拿m的重量，不像抢劫是肆无忌惮的，哈哈~

**解题分析：**
相较HouseRobber，本题正是添加了一个容量为m的限制条件，使得解决问题的难度增加。取数没有HouseRobber相邻元素不能取的限制，这里取数是没有限制的，但是有一个总量的限制。按照递归表达式的写法，有：

$$f(n,m)=max(f(n-1,m-A[n])+A[n],f(n-1,m))$$

之前问题代码表达式也是这个，运行过程中不能很好的限制`A[n]`，当`m<=0`再退出递归时`A[n]`就多加了，当时考虑在递归函数中怎么控制

$$f(n-1,m-A[n])+A[n]$$

一直不能很好的解决，直到从实际含义出发，发现$m-A[n]$是有含义的变量，代表当前背包容量`m`，放进`A[n]`重量的物品后剩下的背包容量，如果这个剩余容量为负数，说明当前元素`n`放不进背包。放不进背包怎么办？那就在当前背包容量下放弃该元素。所以得到结论，如果$m-A[n]$为负数就不能执行$f(n−1,m−A[n])+A[n]$步骤，得到的直接结果直接返回表达式后面的函数，没有`max`的比较过程。
```python
if A[n] > m:
        return helper(n-1,m,A)
else:
    return max(helper(n-1,m-A[n],A)+A[n],helper(n-1,m,A))
```


```python
"""
    递归+暴力搜索：超时
"""
def backPack(m, A):
    '''
    :param m:背包容量
    :param A:元素列表
    :return: 最大取数值
    '''
    if A:
        n = len(A)
        return helper(n-1,m,A)
    return 0
def helper(n,m,A):
    if n < 0 or m <= 0:
        return 0
    if A[n] > m:
        return helper(n-1,m,A)
    else:
        return max(helper(n-1,m-A[n],A)+A[n],helper(n-1,m,A))

"""
    加缓存：超时
"""
def backPack(m, A):
    if A:
        n = len(A)
        cache = [[0] * (m + 1) for i in range(n)]
        return helper(n - 1, m, A, cache)
    return 0
def helper(n, m, A, cache):
    if n < 0 or m <= 0:
        return 0
    if cache[n][m] > 0:
        return cache[n][m]

    if A[n] > m:
        cache[n][m] = helper(n - 1, m, A, cache)
    else:
        cache[n][m] = max(helper(n - 1, m - A[n], A, cache) + A[n], helper(n - 1, m, A, cache))
    return cache[n][m]

"""
    自底向上递推：超内存
"""
def backPack(m, A):
    if A:
        n = len(A)
        dp = [[0]*(m+1) for i in range(n)]
        for i in range(n):
            for j in range(m,-1,-1):
                # cache[n][m] = max(helper(n - 1, m - A[n], A, cache) + A[n], helper(n - 1, m, A, cache))
                if A[i] > j:
                    dp[i][j] = dp[i - 1][j]
                else:
                    dp[i][j] = max(dp[i-1][j-A[i]] + A[i],dp[i-1][j])
        return dp[-1][m]
    return 0

"""
    滚动数组
"""
class Solution:
    def backPack(self, m, A):
        if A:
            n = len(A)
            dp = [0]*(m+1)
            for i in range(n):
                for j in range(m,-1,-1):
                    if A[i] <= j:
                        dp[j] = max(dp[j-A[i]] + A[i],dp[j])
            return dp[m]
        return 0
```

<div align="center"><img src="http://p7erlqn6k.bkt.clouddn.com/image/jpg/python/dp/003-dp.png" height="100%" width="80%"/></div>

该过程如同HouseRobber问题，再现了DP问题的解决步骤。分析状态表达式：

$$f(n,m)=max(f(n-1,m-A[n])+A[n],f(n-1,m))$$

$$dp[i][j]=max(dp[i-1][j-A[i]]+A[i],dp[i-1][j])$$

**含义**：前`n`个元素，背包容量为`m`时可以装载的最大值，等于前`n-1`个元素，容量为`m-A[n]`的背包可以装载的最大容量，加上当前`A[n]`的容量，与前`n-1`个元素，背包容量为`m`时可以装载的最大容量之间的较大值（`n=i,m=j`）

* Min Cost Climbing Stairs

> On a staircase, the i-th step has some non-negative cost cost[i] assigned (0 indexed).Once you pay the cost, you can either climb one or two steps. You need to find minimum cost to reach the top of the floor, and you can either start from the step with index 0, or the step with index 1.

>example:
```
Input: cost = [10, 15, 20]
Output: 15
Explanation: Cheapest is start on cost[1], pay that cost and go to the top.

Input: cost = [1, 100, 1, 1, 1, 100, 1, 1, 100, 1]
Output: 6
Explanation: Cheapest is start on cost[0], and only step on 1s, skipping cost[3].
```

```python
"""
    递归
"""
class Solution:
    def minCostClimbingStairs(self,cost):
        n = len(cost)
        cache = [-1]*(n)
        self.helper(n-1,cost,cache)
        return min(cache[-1],cache[-2])
    def helper(self,n,cost,cache):
        if n < 0:
            return 0
        if cache[n] > -1:
            return cache[n]
        cache[n] = cost[n] + min(self.helper(n-1,cost,cache),self.helper(n-2,cost,cache))
        return cache[n]

"""
    递推
"""
class Solution:
    def minCostClimbingStairs(self, cost):
        n = len(cost)
        dp = [-1]*n
        dp[0],dp[1] = cost[0],cost[1]
        if n>2:
            for i in range(2,n):
                dp[i] = cost[i] + min(dp[i-1],dp[i-2])
        return min(dp[-1],dp[-2])
```

* Best Time to Buy and Sell Stock
```
针对“Best Time to Buy and Sell Stock”问题的总结：
1 如何判断一个问题是不是DP问题？
    常规的套路：最大、最小、最长、种数。并不是可靠的依据
    应该根据DP问题的特征来审视：
        1）是否有子问题最优等同原问题最优
        2）是否有重叠子问题
2 DP只是作为解题的一种方法，不要纠结该问题一定要拿DP解出来，如果其他解法比DP解法更科学更简单，我们为什么还要追求DP问题呢？比如上面的问题
```

* Climbing Stairs
>爬楼梯问题，DP的入门问题。有高度为n的楼梯，现要爬上楼梯，每次可以走1步或者2步，请问爬上高度为n的楼梯一共有多少种爬楼方法。

没有什么好说的，写出状态方程问题就解决了
```
n = 1,ways = 1
n = 2,ways = 2
f(n) = f(n-1) + f(n-2)
前n层楼的爬法 = 前n-1层楼的爬法 + 前n-2层楼的爬法
```

```python
"""
    递归+缓存
"""
class Solution:
    def climbStairs(self,n):
        if n == 1:
            return 1
        cache = [0]*(n+1)
        cache[1],cache[2] = 1,2
        return self.helper(n,cache)
    def helper(self,n,cache):
        if n == 2 or n == 1:
            return cache[2] if n == 2 else cache[1]
        if cache[n] > 0:
            return cache[n]
        cache[n] = self.helper(n-1,cache) + self.helper(n-2,cache)
        return cache[n]
"""
    递推
"""
class Solution:
    def climbStairs(self,n):
        dp = [0] * (n + 1)
        if n == 1:
            return 1
        if n == 2:
            return 2
        dp[1], dp[2] = 1, 2
        for i in range(3, n + 1):
            dp[i] = dp[i - 1] + dp[i - 2]
        return dp[n]
```

* Maximum Subarray（最大子数组和）

>Given an integer array nums, find the contiguous subarray (containing at least one number) which has the largest sum and return its sum.

```
Example:

Input: [-2,1,-3,4,-1,2,1,-5,4],
Output: 6
Explanation: [4,-1,2,1] has the largest sum = 6.
```

首先明确一下，关于最大子数组和问题是有专门的算法来解决[Kadane's Algorithm](https://en.wikipedia.org/wiki/Maximum_subarray_problem)，恰巧该算法用到了缓存和子问题最优化的思想，所以说该解法属于DP问题。但存在的另一个问题是，我们试图理解其状态转移表达式时发现对于`dp[i] = dp[i-1]+A[i]`中`dp[i]`表达的含义不能很好的被说服。下面给出解法并分析关于DP的困惑。

<div align="center"><img src="http://p7erlqn6k.bkt.clouddn.com/image/jpg/python/dp/004-dp.png" height="100%" width="80%"/></div>

上面的图出自老外的一篇[博客](https://www.linkedin.com/pulse/kadanes-algorithm-mustafa-bedir-tapkan)，讲Kadane's Algorithm解决Maximun subarray问题秒杀国内许多小白，从暴力到缓存，值得收藏。

1 网上关于Kadane's Algorithm的解法有两种，其实这两种是同一种思想
```python
'''dp[i-1]>0作为判断条件：'''
if dp[i-1]>0:
    dp[i] = dp[i-1] + A[i]
else:
    dp[i] = A[i]
'''dp[i-1]+A[i]>A[i]作为判断条件：'''
if dp[i-1]+A[i]>A[i]:
    dp[i] = dp[i-1] + A[i]
else:
    dp[i] = A[i]
```

当$dp[i-1]>0$时，不管$A[i]$是正数负数，$dp[i-1]+A[i]>A[i]$是成立的，所以就不要纠结这两种方法了，但是第二种似乎更符合我们的逻辑，因为`dp[i-1]>0`不是什么很好的条件。

2 关于DP问题的困惑：

如果我们定义**`dp[i]`为前`i`个元素中最大子数组的和**，那么太好了，我们完全可以解释状态转换表达式`dp[i] = dp[i-1] + A[i]`为：前`i`个元素的最大子数组和等于前`i-1`个元素的最大子数组和再加上这个`A[i]`，但是我们发现这时的`dp[i]`并不是`maxsubarray`，上图`step3`时`dp[i]=4`，而`maxsubarray=5`。这意味着这种解释说不通`dp[i] = dp[i-1] + A[i]`，至少现在为止我个人没有给出关于转移表达式的一个合理解释。这是关于DP问题的一个困惑，原问题最优解和子问题最优解没有联系。

或许DP的内涵就是`cache`，或许Kadane's Algorithm算法本身是`dp[i] = dp[i-1] + A[i]`，我们不去讨论过多，下面给出解。

```python
"""
    cache[i-1]>0
"""
class Solution:
    def maxSubArray(self, nums):
        if nums:
            n = len(nums)
            if n == 1:
                return nums[0]
            cache = [0]*(n)
            maxsub,cache[0] = nums[0],nums[0]
            for i in range(1,n):
                if cache[i-1]>0:
                    cache[i] = cache[i-1]+nums[i]
                else:
                    cache[i] = nums[i]
                maxsub = max(maxsub,cache[i])
            return maxsub
        return 0

"""
    cache[i-1]+nums[i]>nums[i]
"""
class Solution:
    def maxSubArray(self, nums):
        if nums:
            n = len(nums)
            if n == 1:
                return nums[0]
            cache = [0]*(n)
            maxsub,cache[0] = nums[0],nums[0]
            for i in range(1,n):
                if cache[i-1]+nums[i]>nums[i]:
                    cache[i] = cache[i-1]+nums[i]
                else:
                    cache[i] = nums[i]
                maxsub = max(maxsub,cache[i])
            return maxsub
        return 0
```

* Longest Increasing Subsequence（最长递增子序列）

**子序列只要保证元素先后顺序即可，元素可以是不相邻的，而子数组在保证元素先后顺序同时，所有的元素必须是相邻的**

>Given an unsorted array of integers,find the length of longest increasing subsequence.For example,given [10,9,2,5,3,7,101,18] ,the longest increasing subsequence is [2,3,7,101].Therefore the length is 4.

两种方法解答该题，利用动态规划，维护dp数组记录前i个元素LIS的长度；或者利用一种叫patience sort的思想进行排序。

1 动态规划：
<div align="center"><img src="http://p7erlqn6k.bkt.clouddn.com/image/jpg/python/dp/006-dp.png" height="100%" width="50%"/></div>

DP的核心是根据题目抽象出状态表达式，首先维护一个一维数组dp，你如果问我为什么要首先维护一个dp数组，这个dp相当于缓存，我们尝试将缓存加进解题过程中。dp[i]表示前i个元素的最长递增子序列的长度，并且元素A[i]必定属于这个最长子序列，为什么？因为dp数组从前到后是缓存累加的一种关系，后面的结果需要利用前面的结果，仔细想想所有的DP问题都是这样的。那么，dp[n]表示前n个元素的最长递增子序列的长度，我们假设指针i,i+1,n-2,n-1在A中对应的元素都小于A[n]，这说明第n个元素与前i个、前i+1个、前n-2个、前n-1个元素的最长子序列可以扩展成更长的最长子序列。所以需要在这些符合条件的最长子序列中选取最大值。
```
f(n) = max( f(n),f(i)+1 )       ai<an  0<=i<n
dp[n] = max( dp[n],dp[i]+1 )    ai<an  0<=i<n
```
dp数组初始化全为1，因为在极端情况下，元素降序排列（9876532），此时的LIS长度为1

```python
class Solution:
    def lis(self,nums):
        if nums:
            dp = [1] * len(nums)
            if len(nums) == 1:
                return 1
            lisl = 1
            for i in range(1,len(nums)):
                for j in range(i):
                    if nums[j] < nums[i]:
                        dp[i] = max(dp[i],dp[j]+1)
                lisl = max(lisl,dp[i])
            return lisl
        return 0
```

2 patience sort：什么是耐心？排序？没有听过。它的原理大致是说，依次处理未排序的数组元素，找到符合条件的桶，并将该元素加入桶底，否则依该元素为单位新建一个桶并放入列表。新待处理的元素依次遍历列表中所有的桶，如果该元素小于桶底元素，就将该元素加入该桶底。经过这样处理，列表中桶的个数就是最长递增子序列的长度，而每个桶中最小的元素组合起来组成一个最长递增序列。
<div align="center"><img src="http://p7erlqn6k.bkt.clouddn.com/image/jpg/python/dp/007-dp.png" height="100%" width="50%"/></div>
```python
class Solution:
    def lis(self,nums):
        if nums:
            rst = []
            for i,v in enumerate(nums):
                if rst:
                    isHandle = False
                    for bucket in rst:
                        if v <= bucket[-1]:
                            bucket.append(v)
                            isHandle = True
                            break
                    if not isHandle:
                        rst.append([v])
                else:
                    rst.append([v])
            return len(rst)
        return 0
```

* Perfect Squares（最少完全平方数的个数）

> Given a positive integer n, find the least number of perfect square numbers (for example, 1, 4, 9, 16, ...) which sum to n.

>For example, given n = 12, return 3 because 12 = 4 + 4 + 4; given n = 13, return 2 because 13 = 4 + 9.

动态规划解题，第二种DP对第一种DP进行了改进。为什么DP能够改进？DP改进的过程就是不断加缓存减少时间复杂度的过程。

给定数字n，统计组成n的最少完全平方数的个数。我们怎样统计？按照递归或者递推的思路，n的最少完全平方数个数可以由比n小的数的最少完全平方数个数组成，因为 n = i + j ，我们维护dp数组
```
dp[n] = dp[i] + dp[j] （i+j=n）
```
表示组成n的最少完全平方数的个数=组成i的最少完全平方数的个数+组成j的最少完全平方数的个数。这样，将原问题划分为更小的子问题。为什么这样划分？为什么是i+j=n的形式，不是i+j+k=n的形式？因为i+j = i+(j+k) = n。那么要怎么确定i和j？这就需要把所有的i+j=n的i、j进行一次这样的计算，最后取最小结果。
```
dp[n] = min(dp[n],dp[i]+dp[j]) i+j=n
```
这里min的第一项代表其他i+j计算的结果。这就是状态转移表达式。还要强调一点，初始化dp数组不一定非得全部是INT_MAX值，极端情况下全部由1组成n，所以初始化dp数组为
```python
dp = [i for i in range(n+1)]
```

```python
"""
    leetcode:time limited
"""
class Solution:
    def numSquares(self,n):
        if n > 0:
            if n == 1:
                return 1
            dp = [i for i in range(n+1)]
            for i in range(2,n+1):
                sqroot = math.floor(math.sqrt(i))
                if sqroot * sqroot == i:
                    # 说明该数是一个完全平方数
                    dp[i] = 1
                else:
                    for j in range(1,i//2+1):
                        # 寻找最小的值
                        dp[i] = min(dp[i],dp[j]+dp[i-j])
            return dp[n]
        return 0
```

上面寻找dp[n]=dp[i]+dp[j]从i=1开始到i=n//2有一个遍历过程，这个过程比较耗费时间。怎么优化？对于数字n，可以写成：**普通数字+完全平方数**的形式。我们改写状态表达式为：
```
dp[i+j*j] = min(dp[i+j*j],dp[i]+1)  i+j != n
```
因为一个完全平方数的最少完全平方数的个数为1：dp[j*j] = 1，所以当n被拆分成n=i+j*j的形式时，只需要dp[i]的结果再加上1个完全平方数的结果（1）就行。这就是对之前的DP进行的改进。当然，我们初始化dp数组后可以将所有的完全平方数的dp[i]设置为1
```python
class Solution:
    def numSquares(self, n):
        if n > 0:
            if n == 1:
                return 1
            dp = [i for i in range(n+1)]
            i = 1
            while i*i <= n:
                # 所有的完全平方数初始化为1
                dp[i*i] = 1
                i += 1

            for j in range(1,n):
                k = 1
                while j+k*k <= n:
                    sqroot = math.floor(math.sqrt(j+k*k))
                    if sqroot*sqroot == j+k*k:
                        k += 1
                        continue
                    dp[j+k*k] = min(dp[j+k*k],dp[j]+1)
                    k += 1
            return dp[n]
        return 0
```

上题两种方法都使用了DP，发现虽然状态表达式意义较为清晰，但是只是局部使用DP，由于外部套用for循环，大大降低了效率，增加了时间复杂度。所以并不是所有的题目都适合用DP来解答。

* Counting Bits

> Given a non negative integer number num. For every numbers i in the range 0 ≤ i ≤ num calculate the number of 1's in their binary representation and return them as an array.

```
Example:
For num = 5 you should return [0,1,1,2,1,2]
```

思路：类似斐波纳契数解法，因为$2^m<i<2^n$，维护一个缓存数组`dp`，$dp[i] = dp[i-2^m]+1$，如果$i = 2^m$那么`dp[i]=1`。注意在遍历过程中调整$cache1=2^m$和$cache2=2^n$。

<div align="center"><img src="http://p7erlqn6k.bkt.clouddn.com/image/jpg/python/dp/005-dp.png" height="100%" width="50%"/></div>

```python
class Solution:
    def countBits(self, num):
        if num == 0:
            return [0]
        cache1,cache2 = 1,1<<1
        dp = [0]*(num+1)
        dp[0] = 0
        for i in range(1,num+1):
            if i == cache1:
                dp[i] = 1
            elif i > cache1 and i < cache2:
                dp[i] = dp[i-cache1] + 1
            if i+1 == cache2:
                cache1 = cache2
                cache2 = cache2 << 1
        return dp
```

* Word Break

>Given a non-empty string s and a dictionary wordDict containing a list of non-empty words, determine if s can be segmented into a space-separated sequence of one or more dictionary words.

>Note:(1)The same word in the dictionary may be reused multiple times in the segmentation.(2)You may assume the dictionary does not contain duplicate words.

>example:

```
Input: s = "catsandog", wordDict = ["cats", "dog", "sand", "and", "cat"]
Output: false

Input: s = "applepenapple", wordDict = ["apple", "pen"]
Output: true
Explanation: Return true because "applepenapple" can be segmented as "apple pen apple".Note that you are allowed to reuse a dictionary word.

Input: s = "leetcode", wordDict = ["leet", "code"]
Output: true
Explanation: Return true because "leetcode" can be segmented as "leet code".
```

这道题很重要呀，分词虽然有专门算法实现，但是本题依然考察分词基本原理。

字符串是一长串的，我们如何确定分割的依据？看来只能从wordDict下手，挨个让wordDict中的元素去试，如果匹配上，就匹配下一个。怎样保证wordDict后面的元素也能匹配原始的字符串？使用暴力循环。

下面的代码使用递归+暴力搜索的方式，是“递归+DFS+回溯”同类型代码，因为考虑到每个元素可能匹配到原始字符串的开头，注意这里没有回溯的过程。可想而知，这种方式是timeout的，但是为什么还要写这种方式？**天下武功，在没有想到很好的剑法破解对方的招式时，递归+暴力搜索往往是可以使用的一招。**

```python
"""
    递归+暴力搜索
    timeout
"""
class Solution:
    def wordBreak(self,s,wordDict):
        n = len(wordDict)
        return self.helper(s,wordDict,n)
    def helper(self,s,wordDict,n):
        if not s:
            return True
        for i in range(n):
            if s[:len(wordDict[i])] == wordDict[i]:
                if self.helper(s[len(wordDict[i]):],wordDict,n):
                    return True
        return False
```

这道题可用使用DP来解决。说实话在经历了Perfect Squares题目后，本题最开始还是没有反应过来用DP解决，现在想想，两题还是有相似的地方，在确定`dp[i]`的过程中都要用到`dp[k]（0<=k<i）`的结果。由衷感慨DP问题实在是灵活，抽象程度之高。

我们还是维护一个长度为n+1的dp[]，dp[i]表示前`i`个字符能否被wordDict完全分割，如果可以被完全分割则`dp[i]=True`，否则`dp[i]=Fase`，这时会出现3种情况：

```
s[:i]表示前i个子字符
1 s[:i]恰好是wordDict中的某一个元素   True
2 s[:i]中 s[:k]能够被wordDict完全分割，并且s[k:i]在wordDict中（0<=k<i）    True
3 s[:i]不满足上面两条  False
```

其中第二条就是对原问题的子问题进行的一个研究。这里我们将dp初始化为dp[n+1]长度，将dp[0]初始化为False，然后再对后面的i进行判断。

```python
class Solution:
    def wordBreak(self,s,wordDict):
        if wordDict:
            dp = [False]*(len(s)+1)
            if not s:
                return True
            for i in range(1,len(s)+1):
                # 体会为什么要把指针向后整体移动一位
                if s[:i] in wordDict:
                    dp[i] = True
                    continue
                j = 0
                while j < i:
                    # 注意：这里dp数组和s的数组同用一个指针
                    if dp[j] and s[j:i] in wordDict:
                        dp[i] = True
                        break
                    j += 1
            return dp[i]
        return False
```

这个方法很巧妙，回头再看这段代码只不过是对上面状态转移的3个条件的翻译。难点是从题目要求中抽象出状态转移条件。紧接着，我们看一下Word Break II

* Word Break II

>Given a non-empty string s and a dictionary wordDict containing a list of non-empty words, add spaces in s to construct a sentence where each word is a valid dictionary word. Return all such possible sentences.

>Note:(1)The same word in the dictionary may be reused multiple times in the segmentation.(2)You may assume the dictionary does not contain duplicate words.

>example:

```
Input:
s = "catsanddog"
wordDict = ["cat", "cats", "and", "sand", "dog"]
Output:
[
  "cats and dog",
  "cat sand dog"
]

Input:
s = "pineapplepenapple"
wordDict = ["apple", "pen", "applepen", "pine", "pineapple"]
Output:
[
  "pine apple pen apple",
  "pineapple pen apple",
  "pine applepen apple"
]
Explanation: Note that you are allowed to reuse a dictionary word.

Input:
s = "catsandog"
wordDict = ["cats", "dog", "sand", "and", "cat"]
Output:
[]
```

其实这道题和Word Break要求基本一致，只不过要找出所有可以将字符串完全分割的可能。这种情况下使用“递归+DFS+回溯”的招式是可以解决的，但是同样会出现timeout的问题。
```python
"""
递归+DFS+回溯
"""
class Solution:
    def wordBreak(self, s, wordDict):
        n = len(wordDict)
        temp = []
        rst = []
        self.helper(s,wordDict,n,temp,rst)
        return rst
    def helper(self,s,wordDict,n,temp,rst):
        if not s and temp:
            rst.append(' '.join(temp.copy()))
            return
        for i in range(n):
            if s[:len(wordDict[i])] == wordDict[i]:
                temp.append(wordDict[i])
                self.helper(s[len(wordDict[i]):],wordDict,n,temp,rst)
                temp.pop()
```

有没有其他方法呢？？？待续...

* Maximum Product Subarray

>Given an integer array nums, find the contiguous subarray within an array (containing at least one number) which has the largest product.

```
Input: [2,3,-2,4]
Output: 6
Explanation: [2,3] has the largest product 6.

Input: [-2,0,-1]
Output: 0
Explanation: The result cannot be 2, because [-2,-1] is not a subarray.
```

这道题和Maximum Subarray题类似，那道题是要求最大连续子数组的和，而这道题是要求最大连续子数组的乘积，而乘积问题因为可能有负数出现，两两负数也可能为正，所以情况比较复杂。

下面的解题方法我没有深入去研究为什么要这样做，只是记住在dp_max数组基础上加入了dp_min数组。
```
在 dpmax[i-1]*nums[i]、dpmin[i-1]*nums[i]、nums[i]中寻找最大值和最小值
分别做为dpmax[i]、dpmin[i]的值，使用一个rst保存过程中出现过的最大值。

dpmax[i] = max( dpmax[i-1]*nums[i] , dpcmin[i-1]*nums[i] , nums[i] )
dpmin[i] = max( dpmax[i-1]*nums[i] , dpmin[i-1]*nums[i] , nums[i] )
```

```python
class Solution(object):
    def maxProduct(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        if nums:
            if len(nums) == 1:
                return nums[0]
            locmax,locmin = [0]*len(nums),[0]*len(nums)
            rst,locmax[0],locmin[0] = nums[0],nums[0],nums[0]
            for i in range(1,len(nums)):
                locmax[i] = max(max(locmax[i-1]*nums[i],nums[i]),locmin[i-1]*nums[i])
                locmin[i] = min(min(locmin[i-1]*nums[i],nums[i]),locmax[i-1]*nums[i])
                rst = max(rst,locmax[i])
            return rst
        return 0
```







































































































