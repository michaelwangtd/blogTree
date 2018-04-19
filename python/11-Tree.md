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

```python
# 树节点
class Node(object):
    def __init__(self,data = -1):
        self.data = data
        self.lc = None
        self.rc = None

class BT(object):
    def __init__(self):
        self.root = Node()
        self.queue = []

    # 层次初始化二叉树
    def add(self,nlist):
        for n in nlist:
            node = Node(n)
            if self.root.data == -1:
                self.root = node
                self.queue.append(self.root)
            else:
                tree_node = self.queue[0]
                if tree_node.lc == None:
                    tree_node.lc = node
                    self.queue.append(tree_node.lc)
                else:
                    tree_node.rc = node
                    self.queue.append(tree_node.rc)
                    self.queue.pop(0)

    """
    二叉树遍历方法：
    """
    # 层次遍历
    def level_traverse(self,root):
        rst = []
        queue = []
        if root == None:return
        queue.append(root)

        while queue:
            node = queue.pop(0)
            rst.append(node.data)
            if node.lc:
                queue.append(node.lc)
            if node.rc:
                queue.append(node.rc)
        print('level traverse:',rst)
        return rst

    # 中序遍历
    def middle_traverse(self,root):
        rst = []
        stack = []

        while root or stack:
            while root:
                stack.append(root)# 后续还要遍历右孩子，先将当前节点入栈
                root = root.lc# 继续查找左孩子是否存在
            if stack:# 说明栈顶节点的左孩子为空
                node = stack.pop()# 弹出栈时对节点进行操作
                rst.append(node.data)
                root = node.rc# 更换root为右孩子
        return rst

    # 前序遍历
    # 前序和中序方法一样，只是rst加入的位置不同
    def pre_traverse(self,root):
        rst = []
        stack = []

        while root or stack:# 前序也使用or来连接
            while root:
                rst.append(root.data)
                stack.append(root)# 当前节点先入栈
                root = root.lc
            node = stack.pop()# 说明当前栈顶节点左子树为空
            root = node.rc# 搜索右子树
        return rst

    # 后续遍历
    def back_traverse(self,root):
        rst = []
        stack = []# 只对左、右孩子进行操作
        out_stack = []# out_stack只操作当前节点(入栈的只是当前节点)
        stack.append(root)

        while stack:# 后续只有stack的判断
            node = stack.pop()
            out_stack.append(node.data)
            if node.lc:# 注意为什么先是左孩子
                stack.append(node.lc)# 入的栈是中间栈stack
            if node.rc:
                stack.append(node.rc)
        while out_stack:
            rst.append(out_stack.pop())
        return rst

    # 前序递归
    def pre_recursion(self,root):
        if root==None:
            return
        print(root.data)
        self.pre_recursion(root.lc)
        self.pre_recursion(root.rc)

    # 中序递归
    def middle_recursion(self,root):
        if root==None:
            return
        self.middle_recursion(root.lc)
        print(root.data)
        self.middle_recursion(root.rc)

    # 后续递归
    def back_recursion(self,root):
        if root==None:
            return
        self.back_recursion(root.lc)
        self.back_recursion(root.rc)
        print(root.data)

if __name__ == '__main__':
    btn = [1,2,3,4,5,6]
    # btn = [1,2,3]
    bt = BT()
    bt.add(btn)
    # rst = bt.level_traverse(bt.root)

    bt.middle_traverse(bt.root) #[4, 2, 5, 1, 6, 3]
    bt.pre_traverse(bt.root)    #[1, 2, 4, 5, 3, 6]
    bt.back_traverse(bt.root)   #[4, 5, 2, 6, 3, 1]

    # bt.pre_recursion(bt.root)
    # bt.middle_recursion(bt.root)
    # bt.back_recursion(bt.root)
```

* Binary Sort Tree（Binary Search Tree）
    1. 二叉排序树又，称二叉查找树，亦称二叉搜索树
    
    2. 定义:
        1. 若左子树不为空，则左子树上所有节点的值均小于等于当前根节点的值
        2. 若右子树不为空，则右子树上所有节点的值均大于等于当前根节点的值
        3. 其左右子树也满足这样的性质
        
    3. 二叉排序树生成：

    4. 二叉排序树查找：




```python
    """
        BST定义：
        BST查找：递归、非递归
        前驱、后继：结点x的前驱是指小于x.key的最大关键字的结点；而一个结点x的后继是指大于x.key的最小关键字的结点。
        求后继节点：对于结点x，如果其右子树不为空，那么x的后继一定是其右子树的最左边的结点。而如果x的右子树为空，并且有一个后继，那么其后继必然是x的最底层的祖先，并且后继的左孩子也是x的一个祖先，因此，为了找到这样的后继结点，只需要从x开始沿着树向上移动，直到遇到一个结点，这个结点是它的双亲的左孩子。
        BST插入：
        BST删除：
            1、 如果结点z没有孩子节点，那么只需简单地将其删除，并修改父节点，用NIL来替换z；
            2、 如果结点z只有一个孩子，那么将这个孩子节点提升到z的位置，并修改z的父节点，用z的孩子替换z；
            3、 如果结点z有2个孩子，那么查找z的后继y，此外后继一定在z的右子树中，然后让y替换z。
        BST遍历：
    """
class Node(object):
    def __init__(self,data=-1):
        self.data = data
        self.lc = None
        self.rc = None

class BST(object):
    def __init__(self):
        self.root = None# 初始化根节点为空

    def create_bst(self,datas):
        for data in datas:
            self.add_element(self.root,data)

    def add_element(self,root,data):
        cur = None# nxt为根节点，cur为nxt的父节点为空
        nxt = root# 注意初始两个变量的设置

        while nxt:# while作用找到带插入节点的父节点
            cur = nxt# 立即赋值
            if data<nxt.data:
                nxt = nxt.lc
            else:
                nxt = nxt.rc

        if cur==None:# 初始树为空时cur为空
            self.root = Node(data)
        else:
            if data<cur.data:# 上面只是找到待插入节点父节点，具体待插节点属于左、右子树还得判断
                cur.lc = Node(data)
            else:
                cur.rc = Node(data)

    def search_element(self,data):
        '''
            bst search
        '''
        if self.root==None:
            return None
        tar = self.root
        while tar:
            if data == tar.data:
                return tar.data
            elif data < tar.data:
                tar = tar.lc
            elif data > tar.data:
                tar = tar.rc
        return None
                
    def recursion_search_ele(self,root,data):
        if root==None:
            return None
        if root.data == data:
            return data
        elif data<root.data:
            return self.recursion_search_ele(root.lc,data)
        elif data>root.data:
            return self.recursion_search_ele(root.rc,data)

    # 二叉排序树的遍历-前序
    def pre_recursion(self,root):
        rst = []
        if root==None:
            return None
        else:
            rst.append(root.data)
            if self.pre_recursion(root.lc):
                rst.extend(self.pre_recursion(root.lc))
            if self.pre_recursion(root.rc):
                rst.extend(self.pre_recursion(root.rc))
        return rst
    # 中序遍历
    def middle_recursion(self,root):
        rst = []
        if root==None:
            return None
        else:
            if self.middle_recursion(root.lc):
                rst.extend(self.middle_recursion(root.lc))
            rst.append(root.data)
            if self.middle_recursion(root.rc):
                rst.extend(self.middle_recursion(root.rc))
        return rst

if __name__ == '__main__':
    t = [90,20,25,5,150]
    pre = [90,20,5,25,150]
    middle = [5,20,25,90,150]

    bst = BST()
    bst.create_bst(t)
    print('pre:',bst.pre_recursion(bst.root))
    print('middle:',bst.middle_recursion(bst.root))

    print(bst.search_element(25))
    print(bst.recursion_search_ele(bst.root,50))
    '''
    pre: [90, 20, 5, 25, 150]
    middle: [5, 20, 25, 90, 150]
    25
    None
    '''
```

* 已知某树的中序遍历，前序、后续遍历中的一种，构建该树
> 事实上，知道任意两种方式，并不能唯一地确定树的结构，但是，只要知道中序遍历和另外任意一种遍历方式，就一定可以唯一地确定一棵树
```python

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

* priority queue
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
            见下图
        思考：
            (1)如何实现堆的存储结构？
        堆结构定义：
            init()
            shiftdown()
            shiftup()
        重点：堆与完全二叉树存储结构相同，故可以使用线性结构实现结构存储，通过对指针的操作，实现space O(1)复杂度，同时time O()也会下降
"""
```
![P3 Markdown](http://p3yz9xz5w.bkt.clouddn.com/img/blog/heap1.png "堆的特点")
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


"""
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