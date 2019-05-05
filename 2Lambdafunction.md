# Lambda表达式和匿名函数

Lambda实际上生成一个函数对象，即匿名函数，例如：

```python
f=lambda x,y:x+y

type(f)  	#<class ‘function’>

f(12,34)	#46
```

Lambda表达式基本格式为：

**lambda arg1,arg2,...:<<expression>>**

其中arg1，arg2...是函数的参数，<expression>是函数的语句，其结果为函数的返回值。

匿名函数广泛用于需要函数对象作为参数，函数比较简单且只使用一次的场合。例如：

map()函数，接收一个函数f和一个iterator，并通过f依次作用在iterator的每个元素上，得到一个新的iterator返回。

```python
i=map(lambda x : x*3,(1,2,3))

for t in i: print(t,end=’’)		#3 6 9
```

