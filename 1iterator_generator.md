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

## 	`range`可迭代对象

​		`python3` 中的 `range`等同于 `python2`中的 `xrange`，`python2`中的 `xrange`函数直接在内存中生成数字列表。`python3`中的 `range`是一个可迭代对象，迭代时产生指定范围的数字序列，故可以节省内存空间。

​	`range(start,stop[,step])`

## 	`map` 迭代器 和 `itertools.starmap`迭代器

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

## 	`filter`  迭代器 和 `itertools.filterfalse`迭代器

​		`python3`中的`filter`是迭代器，可以节省内存空间