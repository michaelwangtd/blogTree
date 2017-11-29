### 迭代器 ###
* 凡是可作用于for循环的对象都是Iterable类型；
* 凡是可作用于next()函数的对象都是Iterator类型，它们表示一个惰性计算的序列；
* 集合数据类型如list、dict、str等是Iterable但不是Iterator，不过可以通过iter()函数获得一个Iterator对象；
* Iterator甚至可以表示一个无限大的数据流，例如全体自然数。而使用list是永远不可能存储全体自然数的

### generator（生成器） ###
* 从gensim训练word2vec引入生成器
```python
# 生成器读取大量数据
'''
Gensim中 Word2Vec 模型的期望输入是进过分词的句子列表，即是某个二维数组。这里我们暂时使用 Python 内置的数组，不过其在输入数据集较大的情况下会占用大量的 RAM。Gensim 本身只是要求能够迭代的有序句子列表，因此在工程实践中我们可以使用自定义的生成器，只在内存中保存单条语句。
'''
class MySentences(object):
    def __init__(self, dirname):
        self.dirname = dirname

    def __iter__(self):
        for fname in os.listdir(self.dirname):
            for line in open(os.path.join(self.dirname, fname)):
                yield line.strip()

sentences = MySentences('/some/directory') # a memory-friendly iterator
model = gensim.models.Word2Vec(sentences)
```
* 一些生成器的例子
```python
# 生成器输出fibonacci数列
def fibonacci():
   a = b = 1
   yield a
   yield b
   while True:
     a, b = b, a+b
     yield b

for num in fibonacci():
   if num > 100: break
   print num,

# 1 1 2 3 5 8 13 21 34 55 89
```

**扩展阅读：**
> [Python函数式编程指南（四）：生成器](http://www.cnblogs.com/huxi/archive/2011/07/14/2106863.html)