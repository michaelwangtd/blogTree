
# Part Leetcode Question

* [227 Basic Calculator II](https://leetcode.com/problems/basic-calculator-ii/description/)

```python
class Solution:
    def calculate(self,s):
        op = ['+','-','*','/']
        s = list(s.replace(' ',''))
        if s:
            # '4-3/2'
            # 变量初始化
            num = 0
            operator = '+' # 题目说明所有的元素非负
            # 栈存放正负数，乘除操作在栈外完成
            stack = []
            for i in range(len(s)):
                if s[i].isdigit():
                    num = num * 10 + int(s[i])
                if s[i] in op or i==len(s)-1:
                    if operator in ['+','-']:
                        if operator == '-':
                            num = -num
                        stack.append(num)
                    else:
                        if operator == '*':
                            stack.append(stack.pop() * num)
                        else:
                            tempnum = stack.pop()
                            # 注意弹出的元素有可能是负数，遇到‘/’操作怎么处理
                            if tempnum < 0:
                                temp = -(-tempnum // num)
                            else:
                                temp = tempnum // num
                            stack.append(temp)
                    operator = s[i]
                    num = 0
            while len(stack) > 1:
                num1 = stack.pop()
                num2 = stack.pop()
                stack.append(num1+num2)
            return stack[0]
```

* [55 Jump Game](https://leetcode.com/problems/jump-game/description/)

>进一步思考，我们枚举每一位时都在判断是否被染色过（从而决定是否能够到达该点且能否继续往前走），假设在某一瞬间，index=m 的位置已经被染色了，那么 index=n (n<=m) 的位置肯定已经被染色过了，我们维护一个最右边被染色的点，如果当前枚举点在该点的左侧，那么当前点已经被染色，否则即可停止遍历（因为右边的点再也不可能被染色到了）。

```python
if nums:
            # 最初的最右数组为0
            most_right_index = 0
            for i in range(len(nums)):
                # 如果当前位置位于最右数组的右边，说明到不了当前这个位置
                if i > most_right_index:
                    return False
                # 只需要一个变量存储“最右位置”即可
                most_right_index = max(most_right_index,nums[i]+i)
            return True
        return False
```

* [2 Add Two Numbers](https://leetcode.com/problems/add-two-numbers/description/)

>其实这道题就是大整数加法

```python
class Solution(object):
    def addTwoNumbers(self, l1, l2):
        if l1 and l2:
            root = ListNode(0)
            cur = root
            bit = 0
            while l1 and l2:
                bsum = l1.val + l2.val + bit
                num = bsum % 10
                bit = bsum // 10
                cur.next = ListNode(num)
                cur = cur.next
                l1 = l1.next
                l2 = l2.next
            while l1 and not l2:
                bsum = l1.val + bit
                num = bsum % 10
                bit = bsum // 10
                cur.next = ListNode(num)
                cur = cur.next
                l1 = l1.next
            while l2 and not l1:
                bsum = l2.val + bit
                num = bsum % 10
                bit = bsum // 10
                cur.next = ListNode(num)
                cur = cur.next
                l2 = l2.next
            if bit == 1:
                cur.next = ListNode(bit)
            return root.next
        return None
```

































```python
class Solution:
    def canCompleteCircuit(self,gas,cost):
        if gas:
            mod = len(gas)
            for start in range(len(gas)):
                idx = start
                if gas[idx] >= cost[idx]:
                    earn = gas[idx] - cost[idx]
                    idx = (idx + 1) % mod
                    while idx != start:
                        earn = earn + gas[idx] - cost[idx]
                        if earn < 0:
                            break
                        idx = (idx + 1) % mod
                    if earn >= 0:
                        return start
        return -1
```