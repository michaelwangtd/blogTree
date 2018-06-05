## review ##

* python堆接口调用（实现最大堆、最小堆）
```python
h = [7,3,5,2,1]
## 最小堆
# 初始化最小堆
heapq.heapify(h) # [1, 2, 5, 7, 3]
# 堆中添加元素
heapq.heappush(h,6) # [1, 2, 5, 7, 3, 6]
# 删除堆顶最小元素
mixnum = heapq.heappop(h) # [2, 3, 5, 7, 6]

## 最大堆
h = [2,1,7,4,6]
h = [-item for item in h]
heapq.heapify(h) # [-7, -6, -2, -4, -1]
heapq.heappush(h,-(5)) # [-7, -6, -5, -4, -1, -2]
maxnum = heapq.heappop(h) # [-6, -4, -5, -2, -1]
maxnum = -maxnum # 7
```

* 单例模式
```python
class Singleton(object):
    # _instance是一个类变量
    _instance = None
    # cls指代当前类
    def __new__(cls, *args, **kwargs):
        if not cls._instance:
            cls._instance = super(Singleton,cls).__new__(cls,*args,**kwargs)
        return cls._instance

if __name__ == '__main__':
    s1 = Singleton()
    s2 = Singleton()
    print(s1)
    print(s2)
    print(s1==s2)
    print(id(s1),id(s2))
    """
    <__main__.Singleton object at 0x000001DA6FC8B860>
    <__main__.Singleton object at 0x000001DA6FC8B860>
    True
    2037689923680 2037689923680
    """
```
* 参数\*、\*args、\*kwargs含义
```python
"""“*”拆开列表list的数值作为位置参数，并把位置参数传给函数作为形参来调用"""
"""“*”args接收元组（tuple）作为位置参数"""
"""“**”将字典中的数据项作为键值参数传给函数"""
"""在函数调用中使用“*”，我们需要元组;在函数调用中使用”**”，我们需要一个字典"""
```
* pandas中groupby使用
```python
if __name__ == '__main__':
    pd.set_option('display.width',5000)

    df = pd.DataFrame({'key1':['a','a','b','b','a'],\
                  'key2':['one','two','one','two','one'],\
                  'd1':np.random.randn(5),\
                  'd2':np.random.randn(5)
                  })
    print(df)
    gdk1 = df['d1'].groupby(df['key1'])
    # print(type(gdk1))
    # print(gdk1)
    print(gdk1.mean())
    """
    key1
    a   -0.466118
    b    1.637663
    Name: d1, dtype: float64
    """
    print(gdk1.sum())
    
    gdk2 = df['d2'].groupby([df['key1'],df['key2']])
    # print(gdk2.mean())
    # print(gdk2.mean().unstack())
    """
    key2       one       two
    key1                    
    a    -0.031110 -1.248988
    b    -0.060531 -0.932921
    """
```
* Series（pandas）
```python
	## Series
    """具有数组、字典功能"""
    # 1
    t1 = ['a','b','c','d']
    d1 = Series(t1)
    # print(type(d1))#<class 'pandas.core.series.Series'>
    # print(d1.index)#RangeIndex(start=0, stop=4, step=1)
    # print(d1.values)#['a' 'b' 'c' 'd']
    # print(d1)
    """
    0    a
    1    b
    2    c
    3    d
    dtype: object
    """
    print(d1.isnull)
    t2 = [1,2,3,4]
    d2 = Series(t2,index=t1)
    # print(d2)
    """
    a    1
    b    2
    c    3
    d    4
    dtype: int64
    """
    # 2 使用append连接其他Series
    d3 = Series(t2)
    # print(d3)
    """
    0    1
    1    2
    2    3
    3    4
    dtype: int64
    """
    dt = Series([5,6])
    d4 = d3.append(dt,ignore_index=True)
    # print(d4)
    """
    0    1
    1    2
    2    3
    3    4
    0    5
    1    6
    dtype: int64
    """
    d5 = d4.drop(0)#按照索引index删除
    # print(d5)
    """
    1    2
    2    3
    3    4
    4    5
    5    6
    dtype: int64
    """
```
* DataFrame
```python
    pd.set_option('display.max_columns',None)#全部展示数据

    ## create2csv
    data = {
        'date':pd.date_range('20171101',periods=10),
        'gender':np.random.randint(0,2,size=10),
        'height':np.random.randint(40,50,size=10),
        'weight':np.random.randint(150,180,size=10)
    }
    df = DataFrame(data)
    # df.to_csv('./data.csv')
    ## readfromcsv
    df = pd.read_csv('./data.csv')
    # print(df)
    """
    Unnamed: 0        date  gender  height  weight
    0           0  2017-11-01       0      47     154
    1           1  2017-11-02       1      46     162
    2           2  2017-11-03       0      43     168
    3           3  2017-11-04       1      47     158
    4           4  2017-11-05       1      41     152
    5           5  2017-11-06       0      44     156
    6           6  2017-11-07       1      45     175
    7           7  2017-11-08       0      48     171
    8           8  2017-11-09       1      43     179
    9           9  2017-11-10       0      48     166
    """
    # print(type(df.values))
    # print(df.values)
    """<class 'numpy.ndarray'>
[[0 '2017-11-01' 1 46 160]
 [1 '2017-11-02' 1 45 158]
 [2 '2017-11-03' 0 40 179]
 [3 '2017-11-04' 1 41 151]
 [4 '2017-11-05' 1 44 162]
 [5 '2017-11-06' 0 45 150]
 [6 '2017-11-07' 0 49 172]
 [7 '2017-11-08' 1 48 153]
 [8 '2017-11-09' 1 41 165]
 [9 '2017-11-10' 0 49 178]]"""
    # print(df.index)#RangeIndex(start=0, stop=10, step=1)
    # print(df.index.name)#None

    # print(df.columns)#Index(['Unnamed: 0', 'date', 'gender', 'height', 'weight'], dtype='object')
    # print(df.columns.name)#None

    # print(df.head(2))
    # print(df.tail(2))
    """   Unnamed: 0        date  gender  height  weight
       0           0  2017-11-01       0      49     158
       1           1  2017-11-02       1      40     164
   Unnamed: 0        date  gender  height  weight
       8           8  2017-11-09       0      41     175
       9           9  2017-11-10       0      46     159"""

    # print(df.describe())
    """
           Unnamed: 0     gender     height      weight
    count    10.00000  10.000000  10.000000   10.000000
    mean      4.50000   0.600000  44.200000  159.400000
    std       3.02765   0.516398   3.794733   10.079683
    min       0.00000   0.000000  40.000000  150.000000
    25%       2.25000   0.000000  41.000000  152.500000
    50%       4.50000   1.000000  43.500000  156.500000
    75%       6.75000   1.000000  47.750000  161.750000
    max       9.00000   1.000000  49.000000  178.000000
    """

    # print(df[0:4])
    """
       Unnamed: 0        date  gender  height  weight
    0           0  2017-11-01       0      40     169
    1           1  2017-11-02       0      41     161
    2           2  2017-11-03       0      44     171
    3           3  2017-11-04       0      44     157
    """

    # 按照索引选择数据
    # print(df.loc[:,['gender','height']])
    """
       gender  height
    0       0      47
    1       0      48
    2       1      41
    3       1      43
    4       1      43
    5       0      46
    6       1      43
    7       0      45
    8       1      42
    9       0      49
    """
    # 按照行、列选择数据
    # print(df.iloc[0:4,2:4])
    """
       gender  height
    0       1      44
    1       0      42
    2       0      43
    3       1      44
    """

    # print(df['gender'].value_counts())
    # print(df['height'].median())
    # print(df.mean())
    """
    Unnamed: 0      4.5
    gender          0.5
    height         44.1
    weight        169.6
    dtype: float64
    """
    # print(df['weight'].std())

    # 统计非NA值的个数
    # print(df.count())
    """
    Unnamed: 0    10
    date          10
    gender        8
    height        9
    weight        8
    dtype: int64
    """

    # print(df['gender'])
    """
    0    0.0
    1    1.0
    2    NaN
    3    1.0
    4    NaN
    5    0.0
    6    NaN
    7    NaN
    8    0.0
    9    NaN
    Name: gender, dtype: float64
    """
    # print(df['gender'].dropna())
    """
    0    0.0
    1    1.0
    3    1.0
    5    0.0
    8    0.0
    Name: gender, dtype: float64
    """
    # print(df['gender'].fillna(value=0))
    """
    0    0.0
    1    1.0
    2    0.0
    3    1.0
    4    0.0
    5    0.0
    6    0.0
    7    0.0
    8    0.0
    9    0.0
    Name: gender, dtype: float64
    """

    # 基于行、列频数的统计
    # print(pd.crosstab(df['weight'],df['gender']))
    """
    gender  0.0  1.0
    weight          
    152.0     1    0
    165.0     0    1
    167.0     0    1
    168.0     2    0
    """

    # print(df.drop([0,2,4,6,8], axis=0))  # 删除行，注意原数据不变，返回一个新数据
    """
       Unnamed: 0        date  gender  height  weight
    1           1   2017/11/2     1.0    48.0   165.0
    3           3   2017/11/4     1.0     NaN   167.0
    5           5   2017/11/6     0.0    46.0   168.0
    7           7   2017/11/8     NaN    49.0   164.0
    9           9  2017/11/10     NaN    40.0   162.0
    """

    # print(df.drop(['gender','height','weight'], axis=1))  # 删除列，inplace=True表示直接在原数据修改而不新建对象
    """
       Unnamed: 0        date
    0           0   2017/11/1
    1           1   2017/11/2
    2           2   2017/11/3
    3           3   2017/11/4
    4           4   2017/11/5
    5           5   2017/11/6
    6           6   2017/11/7
    7           7   2017/11/8
    8           8   2017/11/9
    9           9  2017/11/10
    """

    # concat
    """
    相同字段的表首尾相接
    result = pd.concat([df1, df2, df3], keys=['x', 'y', 'z']) //keys给合并的表来源加一个辨识号
    注意多张表concat后可能会出现index重复情况，这是最好使用reset_index重新组织下index。
    result.reset_index(drop=True)
    """

    # 数据数值化 map()
    d = Series(['a','a','b','a','b'])
    # print(d)
    map_dic = {'a':1,'b':0}
    # print(d.map(map_dic))
    """
    0    a
    1    a
    2    b
    3    a
    4    b
    dtype: object
    0    1
    1    1
    2    0
    3    1
    4    0
    dtype: int64
    """
```

* python list 赋值“=”
```python
"""list列表的赋值，复制的是变量内存地址"""
a = ['a','b','c','d','e']
b = a
print(id(a),id(b))# 1898804801096 1898804801096
```

* 如何判断对象为可迭代对象
```python
>>> from collections import Iterable
>>> isinstance('abc', Iterable) # str是否可迭代
True
>>> isinstance([1,2,3], Iterable) # list是否可迭代
True
>>> isinstance(123, Iterable) # 整数是否可迭代
False
```

* enumerate函数特点：将list变成索引-元素树，在for循环中同时迭代索引和元素本身
```python
>>> for i, value in enumerate(['A', 'B', 'C']):
...     print(i, value)
...
0 A
1 B
2 C
```

* 简单总结functools.partial的作用就是，把一个函数的某些参数给固定住（也就是设置默认值），返回一个新的函数，调用这个新函数会更简单

* map()、reduce()函数
> map()函数接收两个参数，一个是函数，一个是Iterable，map将传入的函数依次作用到序列的每个元素，并把结果作为新的Iterator返回。
> Iterator是惰性序列，因此通过list()函数让它把整个序列都计算出来并返回一个list。
> reduce把一个函数作用在一个序列[x1, x2, x3, ...]上，这个函数必须接收两个参数，reduce把结果继续和序列的下一个元素做累积计算

* filter()函数
> 和map()类似，filter()也接收一个函数和一个序列。和map()不同的是，filter()把传入的函数依次作用于每个元素，然后根据返回值是True还是False决定保留还是丢弃该元素。
> 可见用filter()这个高阶函数，关键在于正确实现一个“筛选”函数。
> 注意到filter()函数返回的是一个Iterator，也就是一个惰性序列，所以要强迫filter()完成计算结果，需要用list()函数获得所有结果并返回list。

* python匿名函数
> 关键字lambda表示匿名函数，冒号前面的x表示函数参数。
> 匿名函数lambda x: x * x

* 装饰器：假设我们要增强函数的功能，比如，在函数调用前后自动打印日志，但又不希望修改函数的定义，这种在代码运行期间动态增加功能的方式，称之为“装饰器”（Decorator）。

* 类和实例
```python
"""
# class后面紧接着是类名，即Student，类名通常是大写开头的单词，紧接着是(object)，表示该类是从哪个类继承下来的，继承的概念我们后面再讲，通常，如果没有合适的继承类，就使用object类，这是所有类最终都会继承的类。
# 定义好了Student类，就可以根据Student类创建出Student的实例，创建实例是通过类名+()实现的
# 通过定义一个特殊的__init__方法，在创建实例的时候，就把name，score等属性绑上去
# 注意到__init__方法的第一个参数永远是self，表示创建的实例本身，因此，在__init__方法内部，就可以把各种属性绑定到self，因为self就指向创建的实例本身。
# 和普通的函数相比，在类中定义的函数只有一点不同，就是第一个参数永远是实例变量self，并且，调用时，不用传递该参数
# 和静态语言不同，Python允许对实例变量绑定任何数据，也就是说，对于两个实例变量，虽然它们都是同一个类的不同实例，但拥有的变量名称都可能不同
"""
```

* `variable、_variable、__variable`含义及访问限制
```python
"""
# variable普通变量，\_variable虽然我可以被访问，但是，请把我视为私有变量，不要随意访问，\_\_variable私有变量，不能被访问
# 如果要让内部属性不被外部访问，可以把属性的名称前加上两个下划线\_\_
# 在Python中，实例的变量名如果以\_\_开头，就变成了一个私有变量（private），只有内部可以访问，外部不能访问
# 对于外部代码来说，没什么变动，但是已经无法从外部访问实例变量.\_\_name和实例变量.\_\_score了
# 在Python中，变量名类似__xxx__的，也就是以双下划线开头，并且以双下划线结尾的，是特殊变量，特殊变量是可以直接访问的，不是private变量，所以，不能用__name__、\_\_score__这样的变量名
# \_name，这样的实例变量外部是可以访问的，但是，按照约定俗成的规定，当你看到这样的变量时，意思就是，“虽然我可以被访问，但是，请把我视为私有变量，不要随意访问”
# 双下划线开头的实例变量是不是一定不能从外部访问呢？其实也不是。不能直接访问__name是因为Python解释器对外把__name变量改成了_Student__name，所以，仍然可以通过“\_类名__变量名”来访问“\_\_变量”变量
"""
```

* 类赋值错误示例
```python
"""一种错误示例"""
>>> bart = Student('Bart Simpson', 59)
>>> bart.get_name()
'Bart Simpson'
>>> bart.__name = 'New Name' # 设置__name变量！
>>> bart.__name
'New Name'
"""
这个__name变量和class内部的__name变量不是一个变量！内部的__name变量已经被Python解释器自动改成了_Student__name，而外部代码给bart新增了一个__name变量。
"""
```

* 当子类和父类都存在相同的run()方法时，我们说，子类的run()覆盖了父类的run()，在代码运行的时候，总是会调用子类的run()。
* 在继承关系中，如果一个实例的数据类型是某个子类，那它的数据类型也可以被看做是父类
* 动态语言特点：
```python
"""
# 对于静态语言（例如Java）来说，如果需要传入Animal类型，则传入的对象必须是Animal类型或者它的子类，否则，将无法调用run()方法。
# 对于Python这样的动态语言来说，则不一定需要传入Animal类型。我们只需要保证传入的对象有一个run()方法就可以了
# 这就是动态语言的“鸭子类型”，它并不要求严格的继承体系，一个对象只要“看起来像鸭子，走起路来像鸭子”，那它就可以被看做是鸭子。
"""
```

* 获取对象信息
```python
"""
# 配合getattr()、setattr()以及hasattr()，我们可以直接操作一个对象的状态
# 类似__xxx__的属性和方法在Python中都是有特殊用途的，比如__len__方法返回长度。在Python中，如果你调用len()函数试图获取一个对象的长度，实际上，在len()函数内部，它自动去调用该对象的__len__()方法
# 如果要获得一个对象的所有属性和方法，可以使用dir()函数，它返回一个包含字符串的list
# 能用type()判断的基本类型也可以用isinstance()判断
# 判断基本数据类型可以直接写int，str等，但如果要判断一个对象是否是函数怎么办？可以使用types模块中定义的常量
>>> type(fn)==types.FunctionType
True
>>> type(abs)==types.BuiltinFunctionType
True
>>> type(lambda x: x)==types.LambdaType
True    
"""
```

* 实例属性、类属性
```python
# 给实例绑定属性的方法是通过实例变量，或者通过self变量
s = Student('Bob')
s.score = 90
# 直接在class中定义属性，这种属性是类属性，归Student类所有
class Student(object):
    name = 'Student'
"""
千万不要对实例属性和类属性使用相同的名字，因为相同名称的实例属性将屏蔽掉类属性，但是当你删除实例属性后，再使用相同的名称，访问到的将是类属性
"""
```

* \_\_slots\_\_使用
```python
# 为了达到限制的目的，Python允许在定义class的时候，定义一个特殊的__slots__变量，来限制该class实例能添加的属性
class Student(object):
    __slots__ = ('name', 'age') # 用tuple定义允许绑定的属性名称
```

* @property的使用
* 多重继承
```python
"""
由于Python允许使用多重继承，因此，MixIn就是一种常见的设计。
只允许单一继承的语言（如Java）不能使用MixIn的设计。
"""
class Bat(Mammal, Flyable):
    pass
```

* 枚举类的使用
```python
from enum import Enum, unique

@unique
class Weekday(Enum):
    Sun = 0 # Sun的value被设定为0
    Mon = 1
    Tue = 2
    Wed = 3
    Thu = 4
    Fri = 5
    Sat = 6
"""@unique装饰器可以帮助我们检查保证没有重复值。"""
>>> print(Weekday.Tue)
Weekday.Tue
>>> print(Weekday['Tue'])
Weekday.Tue
>>> print(Weekday.Tue.value)
```

* print()、assert()、logging、pdb
```python
"""启动Python解释器时可以用-O参数来关闭assert"""
def foo(s):
    n = int(s)
    assert n != 0, 'n is zero!'
    return 10 / n
"""logging"""
"""这就是logging的好处，它允许你指定记录信息的级别，有debug，info，warning，error等几个级别，当我们指定level=INFO时，logging.debug就不起作用了。同理，指定level=WARNING后，debug和info就不起作用了。这样一来，你可以放心地输出不同级别的信息，也不用删除，最后统一控制输出哪个级别的信息。"""
import logging
logging.basicConfig(level=logging.INFO)
"""pdb"""
$ python -m pdb err.py
> /Users/michael/Github/learn-python3/samples/debug/err.py(2)<module>()
-> s = '0'
```

* 类内部成员属性的使用
```python
class Ball(object):
    def __init__(self,nums,snum):
        self.nums = nums
        self.snum = snum
        print(self.nums)
        print(self.snum)
    def print_nums(self):
        print(self.nums)
        print(self.snum)
    def del_item(self):
        temp = self.nums
        temp.append('end')
        temp.pop(0)

        t2 = self.snum
        t2 = 'end'
'''
['begin', 1, 2]
begin
[1, 2, 'end']
begin
'''
ball = Ball(['begin',1,2],'begin')
ball.del_item()
ball.print_nums()
'''
在上面的例子中：
    成员变量nums直接赋值给temp，对temp的操作修改后，在另外方法中调用nums变量，其结果也会跟着改变
    而成员变量snum的值赋给t2变量后，对t2的修改并没有影响到原始的snum变量
    原因：
        变量中存储内存地址的，因为同一引用，所以不管是原始变量还是引用的操作都是操作同一内存地址对应的数据
        变量中不存储内存地址的，在变量赋值过程中会申请开辟额外的内存空间被用于新变量的引用
'''
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
    random.random()             生成[0,1)之间随机数
    random.random()*n           生成[0,1*n)==[0,n)之间随机数
    random.random()*(n+1)       生成[0,n+1)之间随机数--->floor()之后[0,n]
    floor(random.random()*n)+1  生成floor([0,n))+1==[0,n-1]+1==[1,n]之间随机数 
"""
# 生成0-n随机数
n = 2
rd = random.random()*(n+1)
print(rd)   # 2.2646389421414552
rd = math.floor(rd)
print(rd)   # 2

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
print(shuffle(t))   #[1, 5, 4, 2, 3]
```

* 不建议使用dict里面的dic.setdefault(k,v)，不安全
```python
dic = {}
rel1 = dic.setdefault(1,'a')
rel2 = dic.setdefault(1,'c')
print(rel1) #a
print(rel2) #a
```
