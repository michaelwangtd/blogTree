* stack and queue in python
```python
# list当做栈使用：
l.append()
l.pop()
# list当做队列使用：
l.append()
l.pop(0)
# 双端队列collection.deque实现队列的作用：
d = deque()
d.popleft()
d.append()
# 双端队列可以操作前后两端：
d.append()
d.pop()
d.appendleft()
d.popleft()
```
* binary tree traverse
```python
class Node(object):
    def __init__(self,data = -1):
        self.data = data
        self.lc = None
        self.rc = None

class BT(object):
    def __init__(self):
        self.root = Node()
        self.queue = []

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

    def pre_recursion(self,root):
        if root==None:
            return
        print(root.data)
        self.pre_recursion(root.lc)
        self.pre_recursion(root.rc)

    def middle_recursion(self,root):
        if root==None:
            return
        self.middle_recursion(root.lc)
        print(root.data)
        self.middle_recursion(root.rc)

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
* binary search tree
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
        nxt = root# 注意出事两个变量的设置

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