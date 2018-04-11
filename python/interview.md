## interview puzzle

* 大整数加法：两个大整数a,b存放在list数组中，其中高位在左边，求两整数之和。 
```python
class Solution(object):
    def sum2bignum(self,a,b):
        '''
            int a = 9999999999
            int b = 11314521
            return int
        '''
        a,b = list(str(a)),list(str(b))
        rst = a if len(a)>len(b) else b
        leng = len(rst)
        i = 0
        bit = 0
        while i<leng:
            abit,bbit = 0,0
            if i<len(a):
                abit = a[-1-i]
            if i<len(b):
                bbit = b[-1-i]
            sbit = int(abit) + int(bbit) + bit
            bit = sbit // 10
            rst[-1-i] = str(sbit % 10)
            i += 1
        if bit == 1:
            temp = [str(bit)]
            temp.extend(rst)
            rst = temp
        return int(''.join(rst))
```