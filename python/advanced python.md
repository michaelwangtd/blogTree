### python相关知识补充 ###
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