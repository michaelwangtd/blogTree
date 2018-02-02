##记录python里面的小技巧和经验用法##

```python
start
```

* 不建议使用dict里面的dic.setdefault(k,v)，不安全
```python
dic = {}
rel1 = dic.setdefault(1,'a')
rel2 = dic.setdefault(1,'c')
print(rel1) #a
print(rel2) #a
```
