# 迭代器

## 可迭代对象

​	实现了 `__iter__()`的对象是可迭代对象。使用内置函数  `iter(obj)`可以调用可迭代对象 `obj`的 `__iter__()`方法，以返回一个迭代器（iterator）。系列对象都是可迭代对象，生成器函数和生成器表达式也是可迭代对象。 `collections.abc`模块中定义了抽象基类`Iterator`。

## 迭代器

​	实现了 `__next__()`的对象是迭代器，可用内置函数 `next（）`调用迭代器的 `__next__()`方法，依次返回下一个项目值；如果没有新项目，则导致`StopIteration`。

​	使用迭代器可以实现对象的迭代循环，迭代器可以使程序更加通用、优雅、高效、更加`Python`化。对于大量项目的迭代，使用列表会占用更多的内存，而使用迭代器可以避免。

## 迭代器协议

迭代器对象必须实现两个方法： `__iter__()`, `__next__()`,二者称为迭代器协议。 `__iter__()`用于返回对象本身，以便for语句进行迭代。 `__next__()`用于返回下一元素。

## 可迭代对象的迭代

### 	`iter`和 `next`函数

​		使用内置函数 `iter(iterator)`可以返回可迭代对象 `iterator`的迭代器。

​		使用内置函数 `next()`,可依次返回迭代器对象的下一项目值，如果没有新项目，则导致`StopIteration`。

### 	while

​		`while`循环也可以循环迭代可迭代对象。

### 	for

​		通常使用for语句实现可迭代对象的迭代。python的for循环实现了自动迭代可迭代对象的功能。

​		`for i in range(10)：pass`

## 自定义可迭代对象和迭代器

​	声明一个类，实现 `__iter__()`和 `__next__()`两个方法。创建该类的对象，即是可迭代对象，也是迭代器。

## 反向迭代：reversed  迭代器

​	使用内置函数 `reversed（）`，可以实现一个系列的反向系列，如果一个可迭代对象实现了 `__reversed__()`方法，则可使用 `reversed（）`函数获得其反向可迭代对象。

​	只有长度有限的系列或实现了 `__reversed__()`方法的可迭代对象，才可以使用内置函数 `reversed（）`。

# 生成器

​	函数定义中，如果使用 `yield` 语句代替 `return` 返回一个值，则定义了一个生成器函数 `（generator）`。

​	生成器函数使用 `yield`语句返回一个值，然后保存当前函数整个执行状态，等待下一次调用。

​	生成器函数是一个迭代器，是可迭代对象，支持迭代。

## 	生成器表达式

​		使用生成器表达式，可以简便快捷地返回一个生成器。生成器表达式的语法和列表解析基本一样，区别在于，生成器表达式使用（）而非列表的 [  ]  。使用列表解析可以简单高效地处理一个可迭代对象，并生成结果列表。

​		`(expr for iter_var in iterator)`	迭代iterator所有内容，计算并返回生成器。

​		`(expr for iter_var in iterator if cond_expr)`		按条件迭代，计算并返回生成器。

​	表达式`expr`使用每次迭代内容 `iter_var`，计算生成一个列表。如果指定了条件表达式 `cond_expr`,则只有满足条件的 `iterator` 元素参与迭代。

​		

```python
	(i**2 for i in range(10))
>>>generator object <genexpr> at 0x028CFBE8
	for x in (i**2 for i in range(10)):
   		 print(x,end=' ')
>>>0 1 4 9 16 25 36 49 64 81
```

# 内置的可迭代对象

## 	range可迭代对象

​		`python3` 中的 `range`等同于 `python2`中的 `xrange`，`python2`中的 `xrange`函数直接在内存中生成数字列表。`python3`中的 `range`是一个可迭代对象，迭代时产生指定范围的数字序列，故可以节省内存空间。

​	`range(start,stop[,step])`

## 	map 迭代器 和 itertools.starmap迭代器

​		`python3`中 `map`是可迭代对象，可节省内存。

​		`map(function,iterator,....)`  map 使用指定函数处理可迭代对象的每个元素。如果需要多个参数，则对应于各可迭代对象。

```python 
list(map(abs,(1,-2,-3)))			#[1 2 3 ]
imoort operator
list(map(operator.add,(1,2,3),(1,2,3)))		#[2,4,6 ]
```

​		如果函数的参数为元组，则需要使用 `itertools.starmap`迭代器：

​		`itertools.starmap(function,iterator)`

```python
import itertools
list(itertools.starmap(pow,[(2,5),(3,2),(10,3)]))		#[32, 9 ,1000 ]
```

## 	filter  迭代器 和 itertools.filterfalse迭代器

​		`python3`中的`filter`是迭代器，可以节省内存空间。

​		`filter(function,iterator)`	#构造函数

​		`filter`使用指定函数处理可迭代对象的每个元素，函数返回`bool`类型的值。若结果为`True`，则返回该元素。如果`function`为`None`，则返回元素为`True`的元素。

```python
filter		#<class 'filter'>
list(filter(lambda x :x>0,(-1,2,-3,0,5)))		#[2,5]
list(filter(None,(1,2,3,0,5)))					#[1,2,4,5]
```

如果需要返回结果为`False`的元素，则需要使用`itertools.filterfalse`迭代器：

​	`filterfalse(predicate,iterator)`	#构造函数

`filterfalse`根据条件函数`predicate`处理可迭代对象的每个元素，若结果为`True`，则丢弃。否则返回该元素。

```python
import itertools
list（itertools.filterfalse(lambda x :x%2,range(10)))		#[0,2,4,6,8]
```

## zip迭代器和zip_longest迭代器

`python3`中的zip是可迭代对象，可以节省内存空间。

`zip(*iterator)`	#构造函数

`zip`拼接多个可迭代对象`iter1，iter2，...`的元素，返回新的可迭代对象，其元素为系列`iter1，iter2，...`对象元素组成的元组。如果各系列长度不一致，则截断至最小系列长度。如果需要截取最大的长度，则使用 `itertools.zip_longest`迭代器：

`zip_longest(*iterator,fillvalue = None)`，其中`fillvalue`是填充值，默认为`None`。

```python
zip		#<class zip>
list(zip((1,2,3),'abc',range(3)))		#[(1,'a',0),(2,'b',1),(3,'c',2)]
list(zip('abc',range(10)))				#[('a',0),('b',1),('c',2)]
list(itertools.zip_longest('ABCD','xy',fillvalue='-'))	#[('A','x'),('B','y'),('C','-'),('D','-')]
```

## enumerate迭代器

`python3`中的`enumerate`是可迭代对象，可以节省内存空间。

`enumerate(iterator,start=0)`	#构造函数

`enumerate`用于枚举可迭代对象`iterator`中的元素，返回元素为元组**（计数，元素）**的可迭代对象。计数从`start`开始（默认为0）。

`list(enumerate('ABCD',start=1000))`		#`[(1000,'A'),(1001,'B'),(1002,'C')，(1003,'D')]`

# iterators模块和迭代器函数

`itertools`模块包含各种迭代器，这些迭代器非常高效，且内存消耗小。`itertools`模块中的迭代器既可以单独使用，也可以组合使用。

## 无穷系列迭代器

`itertools`包含3个无穷系列迭代器：

`count（start = 0，step = 1）`		#从`start`开始，步长为`step`的无穷系列

`cycle（iterator）`							#可迭代对象`iterator`元素的无限重复

`repeat（object[,times]）`				#重复对象`object`无数次（若指定`times`，则重复`times`次）

```python
from itertools import *
list(zip(count(1,2),'abcde'))	#[(1,'a'),(3,'b'),(5,'c'),(7,'d'),(9,'e')]
list(zip(range(10),cycle('abc')))	#[(0,'a'),(1,'b'),(2,'c'),(3,'a'),(4,'b'),(5,'c'),(6,'a'),(7,'b'),(8,'c'),(9,'a')]
list(repeat('god',3))		#['god','god','god']
```

## 累计迭代器accumulate

`itertllos`模块的`accumulate`迭代器用于返回累积和：

​	`accumulate（iterator[,func]）`		

其中，若可迭代对象`iterator`的元素为数值`p1,p2,p3,...`，则结果为：`p1，p1+p2，p1+p2+p3,.....`

如果指定了带两个参数的`func`，则`func`代替默认的加法运算。

```python
import iterator
list(accumulate((1,2,3,4,5)))		#结果元素为:1,3,6,10,15
list(accumulate((1,2,3,4,5),operator.mul))	#使用乘法作为运算，结果为：[1,2,6,24,120]
```

## 级联迭代器chain

`iteratools`模块的`chain`迭代器用于返回级联元素：

​	`chain（*iterators）`		#构造函数

连接所有的可迭代对象`iterator`，作为一个系列返回。

`chain`的类工厂函数`chain.from_iterable(iterator)`，也可用于连接多个系列。

```python
import itertools
list(itertools.chain((1,2,3),'abc',range(5)))		#[1,2,3,'a','b','c',0,1,2,3,4]
list(itertools.chain.from_iterable(['ABC','DEF']))		#['A','B','C','D','E','F']
```

## 选择压缩迭代器compress

用于返回可迭代对象的部分元素：

​	compress（data，selectors）	#构造函数

根据选择器selectors的元素（True/False），返回元素为True对应的data系列中的元素。当data系列或selectors终止时，停止判断。

```python
import itertools
list(itertools.compress('ABCDE',[1,0,1,0,1,1]))		#['A','C','E','F']
```

## 截取迭代器dropwhile和takewhile

`itertools`模块的`dropwhile`和`takewhile`迭代器用于返回可迭代对象的部分元素：

​	`dropwhile(predicate,iterator)`		#`dropwhile`根据条件函数`predicate`处理可迭代对象的每个元素，丢弃`iterator`的元素，直至条件函数的结果为`True`；

​	`takewhile(predicate,iterator)`		#`takewhile`则根据条件函数`predicate`处理可迭代对象的每个元素，返回`iterator`的元素，直至条件函数的结果为`False`。

```python
import itertools
list(itertools.dropwhile(lambda x:x<5,[1,4,6,2,3]))		#[6,2,3]
list(itertools.takewhile(lambda x:x<5,[1,4,6,4,1]))		#[1,4]
```

## 切片迭代器islice

`itertools`模块的`islice`迭代器用于返回可迭代对象的切片

`islice（iterator，stop）`		

`islice（iterator，start，stop[,step]）`

系列支持切片操作，同样，可迭代对象可使用`islice`实现切片功能。`islice`返回可迭代对象iterator的切片，从索引位置`start`（第一个元素为0）开始，到`stop`（不包含）结束，步长为`step`（默认为1）。如果`stop`为`None`，则直至结束。

```python 
import itertools
list(itertools.islice('ABCDEF',2))			 #['A','B']
list(itertools.islice('ABCDEF',2,4))		 #['C','D']
list(itertools.islice('ABCDEF',2,None))		 #['C','D','E','F']
list(itertools.islice('ABCDEF',0,None,2))	 #['A','C','E']
```

## 迭代器groupby

返回可迭代对象的分组：

`groupby（iterator，key=None）`

其中，`iterator`为待分组的可迭代对象。可选的`key`为用于计算键值的函数，默认为`None`，即键值为元素本身值。

`groupby`返回的结果为迭代器，其元素为`（k，group）`，其中`k`是分组的键值，`group`为`iterator`中具有`k`值的元素的集合的**子迭代器**。

```python
import itertools
d=[1,-2,0,0,-1,2,1,-1,2,0,0]
d1=sorted(d,key=abs)
for k,g in itertools.groupby(d1,key=abs):
    print(k,'-----',g)
>>>	0 ----- <itertools._grouper object at 0x000001A519016BA8>
	1 ----- <itertools._grouper object at 0x000001A519016C50>
	2 ----- <itertools._grouper object at 0x000001A519016BA8>
---------------------------------------------------------------------
import itertools
d=[1,-2,0,0,-1,2,1,-1,2,0,0]
d1=sorted(d,key=abs)
for k,g in itertools.groupby(d1,key=abs):
    print(k,'-----',list(g))
>>>	0 ----- [0, 0, 0, 0]
	1 ----- [1, -1, 1, -1]
	2 ----- [-2, 2, 2]
----------------------------------------------------------------------
import itertools
d=[1,-2,0,0,-1,2,1,-1,2,0,0]
d1=sorted(d,key=abs)
for k,g in itertools.groupby(d1):
    print(k,'-----',list(g))
>>>	0 ----- [0, 0, 0, 0]
	1 ----- [1]
	-1 ----- [-1]
	1 -----  [1]
	-1 ----- [-1]
	-2 ----- [-2]
	2 ----- [2, 2]
```

## 返回多个迭代器tee

`iterators`模块的`tee`迭代器用于返回多个可迭代对象：

​	`tee(iterator,n=2)`		

返回可迭代对象`iterator`的`n`个（默认为2）迭代器。

```python
import itertools
for i in itertools.tee(range(10),3):
    print(list(i))
>>>	[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
	[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
	[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
-------------------------------------------------
import itertools
for i in itertools.tee(range(10),3):
    print(i)   
>>>	<itertools._tee object at 0x00000204E993E988>
	<itertools._tee object at 0x00000204E993E9C8>
	<itertools._tee object at 0x00000204E993EA08>
```

## 组合迭代器combinations、combinations_with_replacement

`iterators`模块的`combinations`（元素不重复）和`combinations_with_replacement`（元素可重复）迭代器用于系列的组合：

`combinations（iterator，r）`

`combinations_with_rerplacement(iterator,r)`

返回可迭代对象`iterator`的元素的组合，组合长度为`r`。

```python
import itertools
list(iterator.combinations([1,2,3],2))	#[(1,2),(1,3),(2,3)]
list(iterator.combinations([1,2,3,4],2))	#[(1,2),(1,3),(1,4),(2,3),(2,4),(3,4)]
list(itertools.combinations([1,2,3,4],3))	#[(1,2,3),(1,2,4),(1,3,4),(2,3,4)]
list(itertools.combinations_with_replacement([1,2,3],2))	#[(1,1),(1,2),(1,3),(2,2),(2,3),(3,3)]
```

## 排列迭代器permutations

`itertools`模块的`permutation`迭代器用于系列的排列：

`permutations（iterator，r=None)`

返回可迭代对象`iterator`的元素的排列，组合长度为`r`（默认为系列长度）。

```python
import itertools
list(itertools.permutations([1,2,3],2))		#[(1,2),(1,3),(2,1),(2,3),(3,1),(3,2)]
list(itertools.permutations([1,2,3]))		#[(1,2,3),(1,3,2),(2,1,3),(2,3,1),(3,1,2),(3,2,1)]
```

## 笛卡尔积迭代器

`itertools`模块的`product`迭代器用于系列的笛卡尔积

`product（*iterators，repeat=1）`

返回可迭代对象`iterator1，iterator2，...`的元素的笛卡尔积`，repeat`为可迭代对象的重复次数（默认为1）

```python
import itertools
list(itertools.product([1,2],'abc'))		#[(1,'a'),(1,'b'),(1,'c'),(2,'a'),(2,'b'),(2,'c')]
list(itertools.product([1,2],repeat=3))		#[(1,1,1),(1,1,2),(1,2,1),(1,2,2),(2,1,1),(2,1,2),(2,2,1),(2,2,2)]
```

