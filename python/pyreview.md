##review##
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
