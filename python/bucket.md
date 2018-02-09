## bucket ##
* bucket
```python
# -*- coding:utf-8 -*-

from collections import OrderedDict

def bucket_order(nums):
    '''
    桶是有序的
    :param nums:
    :return:
    '''
    begin,end = min(t),max(t)
    k = list(range(begin,end+1))
    v = [0]*len(k)
    dic = OrderedDict((ki,vi) for ki,vi in zip(k,v))

    for n in nums:
        dic[n] += 1

    rst = []
    for k,v in dic.items():
        while v>0:
            rst.append(k)
            v -= 1
    return rst

def bucket_str_order2(str_list):
    '''

    :param str_list:
    :return:
    '''
    key = ['a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i', 'j',
         'k', 'l', 'm', 'n', 'o', 'p', 'q', 'r', 's', 't',
         'u', 'v', 'w', 'x', 'y', 'z']
    # bucket = OrderedDict.fromkeys(k,[])
    # bucket = OrderedDict({k:[] for k in key})
    bucket = OrderedDict()
    for k in key:
        bucket[k] = []


    for i in range(len(str_list[0])-1,-1,-1):
        for j in range(len(str_list)):
            ki = str_list[j][i]
            bucket[ki].append(str_list[j])
        temp = []
        for k,v in bucket.items():
            if v:
                temp.extend(v)
                bucket[k] = []
        str_list = temp
    return str_list
"""
注意bucket_str_order1和bucket_str_order2两种方法都可用，dic(26个字母)||ord(chr)-ord('a')计算位置
"""

def bucket_str_order1(str_list):
    '''

    :param str_list:
    :return:
    '''
    bucket = []
    for i in range(26):
        bucket.append([])

    for i in range(len(str_list[0])-1,-1,-1):
        for j in range(len(str_list)):
            ki = str_list[j][i]
            bucket[ord(ki)-ord('a')].append(str_list[j])
        temp = []
        for q in range(len(bucket)):
            if bucket[q]:
                temp.extend(bucket[q])
                bucket[q] = []
        str_list = temp
    return str_list


if __name__ == '__main__':
    t = [1,2,2,1,1,4,3,5,5]
    # rst = bucket_order(t)
    # print(rst)  # [1, 1, 1, 2, 2, 3, 4, 5, 5]

    # str_list = ['book','mook','mari','more']
    str_list = ['bad','abc','cod','adc']
    rst = bucket_str_order1(str_list)
    rst = bucket_str_order2(str_list)   #['abc', 'adc', 'bad', 'cod']
    print(rst)

```