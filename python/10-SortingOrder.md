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

* 计数排序

思想：利用空间换时间思想，构建统计器counter统计列表中所有出现元素的次数，元素与counter列表下标一一对应，最后扩展形成结果
```python
    """
    注意：
    不能选取字典为counter，在生成有序字典dic的时候就需要对这个“有序”负责任，
    使用sorted()函数对list排序，这个是不被允许的
    from collections import OrderedDict
    dic = OrderedDict()
    # 获取待排序数组set集合
    setnums = sorted(list(set(nums)))
    """
def counterOrder(nums):
    # 1 list形式连续化初始化counter，即使存在浪费空间的风险
    mnum = max(nums)
    counter = [0]*(mnum+1)
    # 2 遍历统计计数
    for num in nums:
        counter[num] += 1
    # 3 生成有序序列
    nums = []
    for i,v in enumerate(counter):
        if v>0:
            nums.extend([i]*v)
    return nums

```
```python
'测试:'
nums = [1, 2, 3, 3, 2, 1, 4, 5, 4, 3, 2, 1, 4, 6, 5, 4, 3, 3, 67 ,5 ,3, 2]
rst = [1, 1, 1, 2, 2, 2, 2, 3, 3, 3, 3, 3, 3, 4, 4, 4, 4, 5, 5, 5, 6, 67]
```