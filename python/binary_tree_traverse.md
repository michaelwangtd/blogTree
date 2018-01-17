* 
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

        while stack or root:
            while root:
                stack.append(root)
                root = root.lc
            if stack:
                node = stack.pop()
                rst.append(node.data)
                root = node.rc
        print('middle traverse:',rst)
        return rst

    def pre_traverse(self,root):
        rst = []
        stack = []
        while root or stack:
            while root:
                rst.append(root.data)
                stack.append(root)
                root = root.lc
            root = stack.pop()
            root = root.rc
        print('pre_traverse:',rst)

    def back_traverse(self,root):
        rst = []
        stack = []
        out_stack = []
        stack.append(root)
        while stack:
            node = stack.pop()
            out_stack.append(node.data)
            if node.lc:
                stack.append(node.lc)
            if node.rc:
                stack.append(node.rc)
        while out_stack:
            rst.append(out_stack.pop())
        print('back_traverse:',rst)
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