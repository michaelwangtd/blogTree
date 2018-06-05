# Tree

<div align="center"><img src="http://p7erlqn6k.bkt.clouddn.com/image/jpg/python/tree/001-tree.png" height="100%" width="40%"/></div>

* 树的基本概念
    1. 二叉树：
        1. 层数：树的层数从**0**开始计算
            1. **根节点的层数为0**
            2. 当前节点的层数是父节点层数+1
        2. 高度（深度）：树中节点的最大层数
    2. 二叉树性质：
        1. 非空二叉树第i层至多有`2^i`个节点`（i>=0）`
        2. 高度为h的二叉树至多有`2^(h+1)-1`个节点`（h>=0）`
    3. 满二叉树：
        1. 二叉树中所有分支节点的度数为2
    4. 完全二叉树：
        1. 定义：一颗高度为h的二叉树，前h-1层都为满节点状态，第h层节点从左至右排列，空位都在右边
        2. 性质：
            1. n个节点的完全二叉树的高度h为：`h=[log_2(n)]`
            2. n个节点的完全二叉树`（0<=i<=n-1）`
                1. 序号为0的节点是根节点
                2. 节点i的父节点为`(i-1)//2`或者`(i-1)>>2 (i>0)`
                3. `if 2*i+1<n:2*i+1`为当前节点i的左孩子
                4. `if 2*i+2<n:2*i+2`为当前节点i的右孩子
                5. 由于根节点的下标为0，第i层元素从下标`2^i-1`的位置开始存放元素，连续`2^i`个元素属于这一层
        

* Stack And Queue In Python
    1. list当做栈使用：`l.append() l.pop()`
    
    2. list当做队列使用：`l.append() l.pop(0)`

    3. 利用python内置的双端队列：
```python
from collections import deque
dq = deque() # 创建双端队列
# 四种方法
dq.append()
dq.pop()
dq.appendleft()
dq.popleft()
```

* Binary Tree Traverse
>二叉树遍历（Tree Walk）是树结构中的基础问题，具体有前序（Preorder）、中序（Inorder）、后续（Postorder）和层次（Level-order）遍历4中方式，而每种方式都有递归和非递归的实现方法。

<div align="center"><img src="http://p7erlqn6k.bkt.clouddn.com/image/jpg/python/tree/003-tree.png" height="100%" width="40%"/></div>

```python
class BinaryTree(object):
    def __init__(self):
        self.root = None

    def init_complete_binary_tree(self,datas):
        que = []
        for data in datas:
            node = Node(data)
            que.append(node)
            if self.root:
                cur = que[0]
                if cur.left == None:
                    cur.left = node
                elif cur.right == None:
                    cur.right = node
                    que.pop(0)
            else:
                self.root = node

    # 前序递归
    def preorder_recursion(self,root):
        rst = []
        if root == None:
            return rst
        rst.append(root.data)
        rst.extend(self.preorder_recursion(root.left))
        rst.extend(self.preorder_recursion(root.right))
        return rst

    # 中序递归
    def inorder_recursion(self,root):
        rst = []
        if root == None:
            return rst
        rst.extend(self.inorder_recursion(root.left))
        rst.append(root.data)
        rst.extend(self.inorder_recursion(root.right))
        return rst

    # 后续递归
    def postorder_recursion(self,root):
        rst = []
        if root == None:
            return rst
        rst.extend(self.postorder_recursion(root.left))
        rst.extend(self.postorder_recursion(root.right))
        rst.append(root.data)
        return rst

    # 前序非递归
    def preorder_traverse(self,root):
        rst = []
        stack = []
        if root:
            cur = root # 由于是非递归，root是唯一的，所以先要指定一个cur指向root
            while cur or stack: # 如果这两个条件能够同时满足的话，就不需要同时用or判断了
                while cur:
                    rst.append(cur.data)
                    stack.append(cur) # 将cur入栈，因为在后续还要遍历它的右子树
                    cur = cur.left # 当赋值后的cur不能再循环，我们可以知道，当前节点的左子树为空
                if stack:
                    node = stack.pop() # 接上面的cur = cur.left，因为左子树为空，这时要将当前节点弹出
                    cur = node.right # 将右子树赋值给cur
        return rst

    # 中序非递归
    def inorder_traverse(self,root):
        rst = []
        stack = []
        if root:
            cur = root # 由于是非递归，root是唯一的，所以先要指定一个cur指向root
            while cur or stack: # 前序和中序非递归遍历的写法一样，只不过rst.append()加入的时机不同
                while cur:
                    stack.append(cur) # 深度优先将元素先入栈
                    cur = cur.left # 循环不能执行，说明当前元素的左子树为空
                if stack:
                    node = stack.pop() # 弹出栈中元素的时候，说明该元素左子树为空，这就是加入rst的时机
                    rst.append(node.data)
                    cur = node.right # 重新处理右子树
        return rst

    # 后序非递归
    """
        思路：后序反转到栈其实是向右子树的DFS
        推荐这个思路，因为不需要额外记忆后序的原理，只是将向左子树的DFS换成向右子树的DFS，
        最后加上rst的反转就可以完成。
    """
    def postorder_traverse(self,root):
        rst = []
        stack = []
        if root:
            cur = root
            while cur or stack: # 判定条件和其他顺序的条件一样
                while cur:
                    rst.append(cur.data) # 根据逆栈的顺序，这里要先加当前节点
                    stack.append(cur) # 还是先将节点入栈，因为后面有左子树要处理
                    cur = cur.right # 注意这里是cur.right，DFS右子树
                if stack:
                    node = stack.pop()
                    cur = node.left
            # 将栈翻转过来即可
            rst.reverse()
        return rst

    # 后续遍历（保留这个思路）
    def back_traverse(self, root):
        rst = []
        stack = []  # 只对左、右孩子进行操作
        out_stack = []  # out_stack只操作当前节点(入栈的只是当前节点)
        stack.append(root)

        while stack:  # 后续只有stack的判断
            node = stack.pop()
            out_stack.append(node.data)
            if node.lc:  # 注意为什么先是左孩子
                stack.append(node.lc)  # 入的栈是中间栈stack
            if node.rc:
                stack.append(node.rc)
        while out_stack:
            rst.append(out_stack.pop())
        return rst

    # 层次遍历
    def level_traverse(self):
        rst = []
        que = []
        que.append(self.root)
        while que:
            node = que.pop(0)
            rst.append(node.data)
            if node.left:
                que.append(node.left)
            if node.right:
                que.append(node.right)
        return rst

if __name__ == '__main__':

    datas = [4,6,2,1,3,8,7,9,5]
    bt = BinaryTree()
    bt.init_complete_binary_tree(datas)

    rst = bt.level_traverse() # [4, 6, 2, 1, 3, 8, 7, 9, 5]
    # 二叉树的遍历
    rst = bt.preorder_recursion(bt.root) # [4, 6, 1, 9, 5, 3, 2, 8, 7]
    rst = bt.inorder_recursion(bt.root) # [9, 1, 5, 6, 3, 4, 8, 2, 7]
    rst = bt.postorder_recursion(bt.root) # [9, 5, 1, 3, 6, 8, 7, 2, 4]
    rst = bt.preorder_traverse(bt.root) # [4, 6, 1, 9, 5, 3, 2, 8, 7]
    rst = bt.inorder_traverse(bt.root) # [9, 1, 5, 6, 3, 4, 8, 2, 7]
    rst = bt.postorder_traverse(bt.root) # [9, 5, 1, 3, 6, 8, 7, 2, 4]
    print(rst)
```

* Binary Sort Tree（Binary Search Tree）
    1. 二叉排序树，又称二叉查找树，亦称二叉搜索树
    
    2. 定义:
        1. 若左子树不为空，则左子树上所有节点的值均小于等于当前根节点的值
        2. 若右子树不为空，则右子树上所有节点的值均大于等于当前根节点的值
        3. 其左右子树也满足这样的性质
    <div align="center"><img src="http://p7erlqn6k.bkt.clouddn.com/image/jpg/python/tree/002-tree.png" height="100%" width="40%"/></div>
        
    3. 前驱、后继：结点`v`的前驱是指小于`v.key`的最大关键字的结点；而一个结点`v`的后继是指大于`v.key`的最小关键字的结点。例如上图中的节点`4`的前驱为`3`，后继为`5`
    
    4. 如何求一个节点的后继？例如`3`的后继为`4`，而`4`的后继为`5`。
        
        > 对于结点x，如果其右子树不为空，那么x的后继一定是其右子树的最左边的结点。而如果x的右子树为空，并且有一个后继，那么其后继必然是x的最底层的祖先，并且后继的左孩子也是x的一个祖先，因此，为了找到这样的后继结点，只需要从x开始沿着树向上移动，直到遇到一个结点，这个结点是它的双亲的左孩子。

    3. 二叉排序树生成：生成BST的过程也是插入元素的过程，按照当前待插入元素大小，找到其前驱节点或后继节点。依据待插入节点与前驱节点或后继节点的关系，将元素插入到适合的分枝上。

    4. 二叉排序树查找：查找分为递归和非递归的方法。

    5. BST删除？这里后续补充。
    
        > 1、 如果结点z没有孩子节点，那么只需简单地将其删除，并修改父节点，用NIL来替换z；2、 如果结点z只有一个孩子，那么将这个孩子节点提升到z的位置，并修改z的父节点，用z的孩子替换z；3、 如果结点z有2个孩子，那么查找z的后继y，此外后继一定在z的右子树中，然后让y替换z。

```python
class Node(object):
    def __init__(self,data=-1):
        self.data = data
        self.left = None
        self.right = None

class BST(object):
    def __init__(self):
        self.root = None

    # 1 初始化二叉排序树
    def init_bst(self,datas):
        for data in datas:
            self.add_element(data)
    def add_element(self,data):
        node = Node(data)
        # 初始设定cur、nxt的思想要注意
        cur = None
        nxt = self.root
        while nxt:
            cur = nxt # 首先给cur赋值
            if data <= cur.data:
                nxt = cur.left
            else:
                nxt = cur.right
        if cur:
            if data <= cur.data:
                cur.left = node
            else:
                cur.right = node
        else: # 这里只是给root节点赋值
            self.root = node

    # 2 二叉排序树查找 -- traverse
    def search_bst(self,target):
        root = self.root
        if root:
            while root:
                if target == root.data:
                    return target
                if target < root.data:
                    root = root.left
                else:
                    root = root.right
        return -1

    # 3 二叉排序树查找 -- recursion
    def search_recursion(self,target):
        root = self.root
        return self.helper(root,target)
    def helper(self,root,target):
        if root == None:
            return -1
        if target == root.data:
            return target
        elif target < root.data:
            return self.helper(root.left,target)
        else:
            return self.helper(root.right,target)

    # 层次遍历
    def level_traverse(self):
        rst = []
        que = []
        que.append(self.root)
        while que:
            node = que.pop(0)
            rst.append(node.data)
            if node.left:
                que.append(node.left)
            if node.right:
                que.append(node.right)
        return rst

if __name__ == '__main__':
    datas = [5,3,1,4,8,2,9,6]
    bst = BST()
    bst.init_bst(datas) # 初始化二叉查找树
    rst = bst.level_traverse()
    print(rst) # [5, 3, 8, 1, 4, 6, 9, 2]
    # 两种查找方式
    print(bst.search_bst(8))
    print(bst.search_recursion(4))
```

* 二叉树的序列化与反序列化

> 序列化与反序列化：将对象输出到文件，再从文件将对象恢复的过程。

> 序列化与反序列化重点在对象的保存、快速恢复。

>  {3,9,20,#,#,15,7}

<div align="center"><img src="http://p7erlqn6k.bkt.clouddn.com/image/jpg/python/tree/004-tree.png" height="100%" width="20%"/></div>
两个算法都是**利用两个队列level、templevel交替进行层序遍历**的。要注意下面这种树结构，有些算法（据说是阿里一个经验丰富的面试官的代码）在这样情况下恐怕要出错。我的代码进行了修正，将在下面代码处标出。
<div align="center"><img src="http://p7erlqn6k.bkt.clouddn.com/image/jpg/python/tree/005-tree.png" height="100%" width="25%"/></div>

>{3,9,20,#,#,15,7,#,#,#,#,5}

```python
"""序列化"""
def serialize(root):
    rst = []
    if root:
        level = []
        level.append(root)
        # 同时判断level是否全为None：[None,None,None]
        while level and level.count(None) < len(level):
            templevel = []
            for ele in level:
                if ele:
                    rst.append(str(ele.data))
                    templevel.append(ele.left)
                    templevel.append(ele.right)
                else:
                    rst.append('#')
                    templevel.append(None) # 这里要将None加入templevel
                    templevel.append(None)
            level = templevel
        i = len(rst)-1
        while rst[i] == '#':
            i -= 1
        rst = rst[:i+1]
    rst = '{' + ','.join(rst) + '}'
    return rst

"""反序列化"""
def deserialize(datas):
    root = None
    if datas:
        datas = datas.replace('{','').replace('}','').strip().split(',')
        if datas:
            level = []
            templevel = []
            root = Node(datas.pop(0))
            level.append(root)
            while datas: # 外循环是数据
                while level:
                    cur = level.pop(0)
                    if datas:
                        data = datas.pop(0)
                        node = None if data == '#' else Node(int(data))
                        if cur: # 如果cur为空就不能添加后代节点
                            cur.left = node
                        templevel.append(node)
                    if datas:
                        data = datas.pop(0)
                        node = None if data == '#' else Node(int(data))
                        if cur:
                            cur.right = node
                        templevel.append(node)
                    if not datas: # 这个判断只是跳出内循环
                        break
                level = templevel
                templevel = []
    return root
```

* 求二叉树的最大深度和最小深度
>有必要明确一下深度的概念。深度也叫高度，是一个二叉树层数的最大值，根节点的层数为0，这样来定义的话，下面的一个二叉树的高度（深度）为3。不知为什么，lintcode上下面这颗二叉树的最大深度为4，最小深度为1。不过不影响算法的计算过程，仅仅是初始化depth=0还是depth=1。

<div align="center"><img src="http://p7erlqn6k.bkt.clouddn.com/image/jpg/python/tree/006-tree.png" height="100%" width="25%"/></div>
> {3,#,20,#,#,15,7,#,#,#,#,5}

这里的代码也是利用level和templevel两个队列交替来层次遍历。看来这是一个可靠的方法。
```python
'''最大深度'''
def maxdepth(root):
    depth = 0
    if root:
        level = []
        templevel = []
        level.append(root)
        while level:
            node = level.pop()
            if node.left:
                templevel.append(node.left)
            if node.right:
                templevel.append(node.right)
            if not level:
                depth += 1
                level = templevel
                templevel = []
    return depth

'''最小深度'''
def mindepth(root):
    depth = 0
    if root:
        level = []
        templevel = []
        level.append(root)
        while level:
            node = level.pop(0)
            # 一旦有子节点为None，立马停止循环，并输出
            if not node.left or not node.right:
                depth += 1
                break
            if node.left:
                templevel.append(node.left)
            if node.right:
                templevel.append(node.right)
            if not level:
                depth += 1
                level = templevel
                templevel = []
    return depth

```

* 最近公共祖先（Lowest Common Ancestor）

> 已知根节点root和两个元素num1,num2，找出两个元素的最近公共祖先

> 递归

```python
def lowest_common_ancestor(root,num1,num2):
    if root:
        if root.data == num1 or root.data == num2:
            return root
        node1 = lowest_common_ancestor(root.left,num1,num2)
        node2 = lowest_common_ancestor(root.right,num1,num2)
        if node1 and node2:
            return root
        else:
            return node1 if node1 else node2
    return None
```

* 已知某树的中序遍历，前序、后续遍历中的一种，构建该树
> 事实上，知道任意两种方式，并不能唯一地确定树的结构，但是，只要知道中序遍历和另外任意一种遍历方式，就一定可以唯一地确定一棵树

1 已知前序和中序，构建二叉树
> 前序[5,3,1,2,4,6,7]

> 中序[1,2,3,4,5,6,7] 
> <div align="center"><img src="http://p7erlqn6k.bkt.clouddn.com/image/jpg/python/tree/009-tree.png" height="100%" width="30%"/></div>

```python
"""
前序+中序
"""
def build_tree(preorder,inorder):
    if preorder:
        # 注意end指针取值
        pre_start,pre_end,in_start,in_end = 0,len(preorder),0,len(inorder)
        return helper(preorder,pre_start,pre_end,inorder,in_start,in_end)
    return None

def helper(preorder,pre_start,pre_end,inorder,in_start,in_end):
    # 判断的是中序的start和end
    if in_start == in_end:
        # 因为end本来就多出一位，如果start==end，说明这个子树已经没有元素了
        return None
    mid = in_start
    while mid < in_end:
        if inorder[mid] == preorder[pre_start]:
            break
        mid += 1
    root = Node(inorder[mid])
    # 边界取值规律为：左闭右开，所以end取值要+1
    root.left = helper(preorder,pre_start+1,pre_start+mid-in_start+1,inorder,in_start,mid)
    root.right = helper(preorder,pre_start+mid-in_start+1,pre_end,inorder,mid+1,in_end)
    return root
# {5,3,6,1,4,#,7,#,2}
```

2 已知后续与中序，构建二叉树
> 前序[2,4,1,3,5]

> 中序[4,2,5,3,1] 
> <div align="center"><img src="http://p7erlqn6k.bkt.clouddn.com/image/jpg/python/tree/010-tree.png" height="100%" width="25%"/></div>

注意体会这里的代码，因为中序的规律是不变的，始终“左子树+root+右子树”。而后序序列为“左子树+右子树+root”，由于root在后面不容易计算，这里使用一个技巧，在算法开始前先将postorder后续进行一个**翻转**操作reverse，这样postorder的顺序变成“root+右子树+左子树”。另外，翻转后这里要体会右子树post_end和左子树post_start变量的计算，这里使用**“-”减法**进行计算。
```python
"""
后序+中序
"""
def build_tree(postorder,inorder):
    if postorder:
        # 注意这里的翻转操作，这个操作来源二叉树非递归后序遍历的做法
        postorder.reverse()
        # end 指针同样+1
        post_start,post_end,in_start,in_end = 0,len(postorder),0,len(inorder)
        return helper(postorder,post_start,post_end,inorder,in_start,in_end)
    return None

def helper(postorder,post_start,post_end,inorder,in_start,in_end):
    # 判断的是中序的start和end
    if in_start == in_end:
        return None
    mid = in_start
    while mid < in_end:
        if inorder[mid] == postorder[post_start]:
            break
        mid += 1
    root = Node(inorder[mid])
    # 注意这里right:post_end和left:post_start的计算使用了减法：post_end-(mid-in_start)
    root.right = helper(postorder,post_start+1,post_end-(mid-in_start),inorder,mid+1,in_end)
    root.left = helper(postorder,post_end-(mid-in_start),post_end,inorder,in_start,mid)
    return root
# {1,2,3,#,4,#,5}
```

二叉查找树的一个应用，二叉查找树区间搜索

* 现给定二叉查找树的根节点root和两个值k1,k2，其中`k1 < k2`，找到所有满足`k1< key< k2`条件的节点的key值，并按照升序返回
> 如下图所示二叉查找树，k1=10,k2=22 返回：[12,20,22] 
> <div align="center"><img src="http://p7erlqn6k.bkt.clouddn.com/image/jpg/python/tree/011-tree.png" height="100%" width="25%"/></div>

```python
def search_range(root,k1,k2):
    rst = []
    helper(rst,root,k1,k2)
    rst = sorted(rst)
    return rst
def helper(rst,root,k1,k2):
    if root == None:
        return
    if k1 <= root.data and root.data <= k2:
        rst.append(root.data)
    '''
    # 下面的判断与上面的判断不重复
    # 下面分开判断，只判断左、右分支是否可能存在符合条件的节点，符合条件的就递归
    # 而具体是否符合条件，由上面具体的条件进行判断
    '''
    if k1 <= root.data:
        helper(rst,root.left,k1,k2)
    if root.data <= k2:
        helper(rst,root.right,k1,k2)
```

* Priority Queue
```python
"""
    优先队列：
        定义：数据的一种存储结构，存储在其中的元素都有一个表示该元素优先程度的权重值。该结构要保证在任何时候访问或弹出的元素是当前结构中优先程度最高的元素。
        思考：
            (1)使用什么结构来实现上面定义的功能？heap（堆，树形结构）
            (2)什么是堆？堆的定义是什么？堆的特点？
    堆：
        定义：堆是一种树形结构，是节点里存储数据的完全二叉树，另外堆中的节点要满足一定的堆序
        堆的特点：
            (1)满足一定的堆序，最小堆？最大堆？
            (2)堆是一颗完全二叉树
            (3)堆的存储结构使用数组/list来实现
        思考：
            (1)如何实现堆的存储结构？
                使用数组来实现。为什么能用数组实现？堆的结构同完全二叉树，由于完全二叉树的相关性质，其存储结构可以利用线性（数组）结构实现，故堆也可以由线性结构的数组来实现
        堆与完全二叉树区别？
            存储形式上没有区别，都用数组来实现
            但堆区别于完全二叉树，要时刻满足堆序
        堆与二叉查找树的区别？
            二叉查找树：
                (1)不一定是完全二叉树，可以是其他二叉树
                (2)左子树<=当前节点，当前节点<=右子树
            堆：
                (1)必须是完全二叉树
                (2)堆序：
                    最小堆：当前节点值小于等于孩子节点值
                    最大堆：当前节点值大于等于孩子节点值
        堆结构定义：
            init()
            shiftdown()
            shiftup()
        堆的操作：
            (1)初始化：
                所有元素先全部加入队列
                从n//2节点开始下沉每个元素，shiftdown
            (2)添加一个元素：
                添加元素到末尾
                上浮shiftup元素
            (3)取出一个元素：
                取出heap[0]
                将heap[-1]放置到heap[0]
                下沉元素

        重点：堆与完全二叉树存储结构相同，故可以使用线性结构实现结构存储，通过对指针的操作，实现space O(1)复杂度，同时time O()也会下降
"""
```

<div align="center"><img src="http://p7erlqn6k.bkt.clouddn.com/image/jpg/python/tree/012-tree.png" height="100%" width="30%"/></div>

```python
# 优先队列 + 堆 的实现
"""
    由于自身特点原因，完全二叉树的结构使用线性结构实现，python使用list
    堆的结构同完全二叉树，堆的实现也借助线性结构
    优先队列的实现依靠堆封装
"""

class Heap(object):
    def __init__(self,elems=None):
        self.elems = list(elems)

    def get_heap(self):
        return self.elems

    def init_heap(self):
        '''
           注意：初始化的时候从len(elems)//2的索引处开始，因为该索引向后的节点都认为已经是一个堆了
        '''
        elems = self.elems
        leng = len(elems)
        begin = leng//2
        for i in range(begin,-1,-1):    # 注意2：初始化的时候也要对根节点进行下沉操作
            e = elems[i]
            self.shiftdown(e,i,leng)

    def pop_top(self):
        elems = self.elems
        rst = elems[0]

        e = elems.pop()
        leng = len(elems)

        self.shiftdown(e,0,leng)
        return rst

    def shiftdown(self,e,father,end):
        elems,father,child = self.elems,father,(2*father)+1

        while child < end:
            if child +1 < end and elems[child] > elems[child+1]:    # 注意1：这里要判断child+1的范围
                child += 1
            if e < elems[child]:
                break
            elems[father] = elems[child]

            father,child = child,(2*child)+1
        elems[father] = e

    def add(self,e):
        elems = self.elems
        elems.append(e)

        leng = len(elems)
        self.shiftup(e,leng-1)

    def shiftup(self,e,end):
        elems,child,father = self.elems,end,(end-1)//2

        while child>0:
            if e < elems[father]:
                elems[child] = elems[father]

                child,father = father,(father-1)//2
        elems[child] = e

class PriorityQueue(object):
    def __init__(self,elems):
        self.heap = Heap(elems)

    def get_queue(self):
        self.heap.get_heap()

    def init_queue(self):
        self.heap.init_heap()

    def pop(self):
        self.heap.pop_top()
```

* 第二次实现堆结构总结：
> 下面代码是第二次实现堆的代码，代码是自己根据原理没有参考其他资料（shiftdown参考了书上的思路）实现的，代码功能没有问题。

* 代码的问题：
    1. 初始化堆同add one代码一样，都是一个一个加入。这里要注意，书上标准的堆在初始化时是全部加入数组，从(n//2)开始执行下沉操作的； 
    2. 代码中频繁使用self.heap操作，标准写法中用临时变量heap，不经常使用self.heap； 
    3. 上浮和下沉操作，标准代码结构更完善，不使用swap，效率更高；
    4. 注意下沉shiftdown代码while部分对最小子节点选取的思路。

贴下代码，供比较
```python
class Heap(object):
    def __init__(self):
        self.heap = []
    def init_heap(self,datas):
        if datas:
            for data in datas:
                self.add_element(data)
    def add_element(self,data):
        self.heap.append(data)
        self.shiftup()

    def add_one(self,data):
        self.heap.append(data)
        self.shiftup()

    def pick_one(self):
        if self.heap:
            data = self.heap[0]
            self.heap[0] = self.heap[-1]
            self.heap.pop()
            if self.heap:
                self.shiftdown()
            return data
        return -1

    def shiftup(self):
        pos = len(self.heap)-1
        while (pos-1)//2 >= 0:
            temp = (pos-1)//2
            if self.heap[pos] < self.heap[temp]:
                self.heap[pos],self.heap[temp] = self.heap[temp],self.heap[pos]
                pos = temp
            else:
                break

    def shiftdown(self):
        if self.heap:
            cur = 0
            # while 2*cur+1<len(self.heap) or 2*cur+2<len(self.heap):
            # 下面的代码解决了上面代码的问题，先判断一个，符合条件后再判断另外一个
            # 下面的代码是参考书上的思路
            while 2*cur+1<len(self.heap):
                temp = 2*cur+1
                if temp+1<len(self.heap) and self.heap[temp+1] < self.heap[temp]:
                    temp = temp + 1
                if self.heap[temp] >= self.heap[cur]:
                    break
                self.heap[cur],self.heap[temp] = self.heap[temp],self.heap[cur]
                cur = temp
```

ajdsjf




```python
"""
    堆排序：
    注意：
        1 需要明确，堆的功能就是确保每次弹出堆顶的元素是当前堆中权重的最值，对应小顶堆中，每次弹出的元素都是当前堆中的最小值
        2 弹出的元素需要存放，在不借助额外空间的情况下，将弹出的元素存放到线性结构对应尾部
        3 情况2的结果会造成线性结构从左至右是降序的，在这种情况下需要以O(n)再遍历一遍线性结构
"""
def heap_order(nums):
    # 初始化堆
    pass
    # order

if __name__ == '__main__':
    nums = [4, 2, 6, 3, 2, 1]
    ## 堆排序
    heap_order(nums)
    exit(0)
    ## 堆的测试
    heap = Heap(nums)
    heap.init_heap()
    print(heap.get_heap())  #[1, 2, 4, 3, 2, 6]

    top_num = heap.pop_top()
    print(top_num)  # 1
    print(heap.get_heap()) # [2, 2, 4, 3, 6]

```


* 字典树Trie Tree
```python
#trie tree demo
class DFA(object):
    _root = dict()

    def init_tree(self,wordlist):
        if wordlist:
            for word in wordlist:
                self.add_word(word)

    def add_word(self,word):
        root = self._root
        for char in word:
            if char not in root:
                root[char] = dict()
            root = root.get(char)
        root['end'] = None

    def filter(self,prestr):
        if prestr:
            prestr = list(prestr)
            i = 0
            while i<len(prestr):
                start = i
                end = self.search_tree(start,prestr)
                if start==end:
                    i += 1
                else:
                    prestr[start:end] = '*'*(end-start)
                    i = end
            return ''.join(prestr)

    def search_tree(self,start,prestr):
        root = self._root
        end = temp = start

        while root.get(prestr[temp]):
            root = root.get(prestr[temp])
            temp += 1
            if 'end' in root:
                end = temp
                break
        return end

if __name__ == '__main__':
    siwords = [
        '法轮功',
        '法轮大法',
        '中国共产党',
        '中共',
        '上海帮',
        '日本AV'
    ]
    prestr = '法日本AV我们知道法轮功是反中共的有力武器'

    print(prestr)
    dfa = DFA()
    dfa.init_tree(siwords)
    rst = dfa.filter(prestr)
    print(rst)
    """
    法轮日本AV我们知道法轮功是反中共的有力武器
    法轮****我们知道***是反**的有力武器
    """
```
