##linkedlist##
* 寻找linkedlist中点
```java
ListNode* findMidNode(ListNode* head)  
{  
       ListNode *slow, *fast;  
       slow = head;  
       fast = head;  
        
       while (fast && fast->next)  
       {  
              slow = slow->next;  
              fast = fast->next-next;  
       }  
        
       return slow;  
       //这里注意的是，如果fast非空，说明  
      //奇数个节点，slow就是中间那个节点  
     //如下fast为空，那么slow就是是线偏右      //的那个节点  
  
} 
```
这里写了一个python版本的实现：
```python
# t = [1,2,3,2,1]
# t = [1,2,2,1]
# t = [1,1]
# t = [1]

def detect_node(head):
    fast = slow = head
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
    return slow
```