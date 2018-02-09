##记录python里面的小技巧和经验用法##
* demo
```python
start
```

* 移位运算的作用
```python
"""
    >> 每右移一位除以2
    << 每左移一位乘以2
    这样的操作瞬间使得代码高大上
"""
print((5-1)<<1) # 8
print((5-1)>>1) # 2
print((4-1)>>1) # 3除以2=1.5 结果为1
print(3/2)  # 1.5
print(3//2) # 1
```

* 随机数 and shuffle()
```python
"""
	random.random()				生成[0,1)之间随机数
	random.random()*n			生成[0,1*n)==[0,n)之间随机数
	random.random()*(n+1)		生成[0,n+1)之间随机数--->floor()之后[0,n]
	floor(random.random()*n)+1	生成floor([0,n))+1==[0,n-1]+1==[1,n]之间随机数	
"""
# 生成0-n随机数
n = 2
rd = random.random()*(n+1)
print(rd)	# 2.2646389421414552
rd = math.floor(rd)
print(rd)	# 2

# 自定义shuffle算法
t = [1,2,3,4,5]

def shuffle(l):
    leng = len(l)
    for i in range(leng):
        j = i
        while i == j:
            j = math.floor(random.random()*leng)
        l[i],l[j] = l[j],l[i]
    return l
print(shuffle(t))	#[1, 5, 4, 2, 3]
```

* 不建议使用dict里面的dic.setdefault(k,v)，不安全
```python
dic = {}
rel1 = dic.setdefault(1,'a')
rel2 = dic.setdefault(1,'c')
print(rel1) #a
print(rel2) #a
```
