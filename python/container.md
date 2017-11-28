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