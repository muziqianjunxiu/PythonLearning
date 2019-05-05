# **functools模块和函数工具**

标准库functools提供了若干关于函数的函数，这些工具函数以函数作为参数，返回修改后的函数。



**1.@functools.wraps** **装饰器**

函数包含若干属性，如__name__、__doc__等。函数被装饰后其特殊属性将变成相应装饰器函数的特殊属性内容，当使用反射时，可能会导致意外结果。使用functools模块的wraps装饰器用于解决该问题。使用wraps装饰函数foo，则foo装饰的函数的特殊属性保留。

​	**@functools.wraps( wrapped,assigned = WRAPPER_ASSIGNMENTS, updated = WRAPPER_UPDATES )**

其中assigned为元组，用于指定使用的wrapped函数的特殊属性，默认为functools模块常量；updated为元组，用于指定更新的wrapped函数的特殊属性。

```python
>>>import  functools
>>>functools.WRAPPER_ASSIGNMENTS
		( ‘__module__’ , ‘__name__’ , ‘__qualname__’ , ‘__doc__’ , ‘__annotations__’)
>>>functools.WRAPPER_UPDATES
		(‘__dict__’)
```

示例：

```python
import time functools
def foo(f):
	‘’’foo function doc’’’
	@functools.wraps(f)
	def wrapper(*x,**y):
		print(‘调用函数：’, f.__name)
		return f( *x,**y)
	return wrapper
@foo
def bar(x):
	‘’’bar function doc’’’
	return x**2
if __name__==’__main__’:
	print(bar(2))
	print(bar.__name__)
	print(bar.__doc__)
    
>>>调用函数：bar
	4
	bar
	bar function doc
```

如果没有加wraps装饰，则效果是：

```python
import time functools
def foo(f):
	‘’’foo function doc’’’
	def wrapper(*x,**y):
		‘’’wrapper function doc’’’
		print(‘调用函数：’, f.__name)
		return f( *x,**y)
	return wrapper
@foo
def bar(x):
	‘’’bar function doc’’’
	return x**2
if __name__==’__main__’:
	print(bar(2))
	print(bar.__name__)
	print(bar.__doc__)
    
>>>调用函数：bar
	4
	wrapper
	wrapper function doc
```





**2.functools.update_wrapper** **函数**

使用update__wrapper函数也可以指定wrapper函数使用指定的特殊属性。

​	**functools.update_wrapper( wrapper,wrapped,assogned = WRAPPER_ASSIGNMENTS,update = WRAPPER_UPDATES)**

示例：

```python
import time functools
def foo(f):
	‘’’foo function doc’’’
	def wrapper(*x,**y):
		‘’’wrapper function doc’’’
		print(‘调用函数：’, f.__name)
		return f( *x,**y)
	return functools.update_wrapper(wrapper,f)
@foo
def bar(x):
	‘’’bar function doc’’’
	return x**2
if __name__==’__main__’:
	print(bar(2))
	print(bar.__name__)
	print(bar.__doc__)
    
>>>调用函数：bar
	4
	bar
	bar function doc
```





**3.functools.total_ordering** **装饰器**

​	支持大小比较的对象需要实现特殊方法：__eq__、__lt__、__le__、__ge__、__gt__。使用functools模块的total_ordering装饰器装饰类，则只需要实现__eq__,以及__lt__、__le__、__ge__、__gt__中的任意一个即可。

示例：

```python
>>>from functools import total_ordering
@total_ordering
class Door(object):
​    def __init__(self):
​        self.value = 0
​        self.first_name = ''
​        self.last_name = ''
​    def __eq__(self, other):
​        print '=== my eq==='
​        return (self.first_name, self.last_name) == (other.first_name, other.last_name)
​    def __gt__(self, other):
​        print '=== my total_ordering==='
​        return (self.first_name, self.last_name) > (other.first_name, other.last_name)
a = Door()
b = Door()
a.first_name = 'ouyang'
a.last_name = 'guoge'
b.first_name = 'aaaa'
b.last_name = 'bbbb'
print a == b
print a > b
print a < b
print a <= b
print a >= b
>>> === my eq===
​    False
​    === my total_ordering===
​    True
​    === my total_ordering===
​    False
​    === my total_ordering===
​    False
​    === my total_ordering===
​    True
```





**4.@functools.lru_cache** **装饰器**

​	可以缓存最多maxsize谷歌此函数的调用结果，从而提高程序执行效率，特别适合耗时的函数或i/o函数

​		**@functools.lru_cache(maxsize=128,typed=False)**

​	maxsize为最大缓存次数，为None则无限制，设置为2的n次方时性能最佳。若typed为True，则不同参数类型的调用将分别缓存，例如f(3)和f(3.0)。





**5.partial** **对象**

​	重新绑定函数的可选参数，生成一个可调用（callable）的partial对象：

​		**functools.partial( func , *args , ******keywords)**

​	其中func是函数，args是其位置参数，keywords是关键字参数

​	partial主要用于设置预先已知的参数，从而减少调用时传递参数的个数。示例：

```python
import functools,math
pow2 = functools.partial( math.pow,2)     #封装pow（x，y[，z]），指定参数x=2
for i in range(11):
print(format(pow2(i), ‘0.0f’))
>>>1 2 4 8 16 32 64 128 256 512 1024
```

**6.reduce** **函数**

reduce使用指定的带两个参数的函数func，对一个数据集合（可迭代对象）的所有数据进行下列操作：使用第1,2个数据作为参数用func函数运算，得到的结果再与第3个数据作为参数用func函数运算，以此类推最后得到一个结果。可选的initializer为初始值。

​		**functools.reduce(func,iterator [,initializer])**

```python
import functools operator
functools.reduce(operator.add,[1,2,3,4,5])    		#((((1+2)+3)+4)+5)=15
>>>15
import functools operator
functools.reduce(operator.add,[1,2,3,4,5],10)       #10+((((1+2)+3)+4)+5)=25
>>>25
```

 

 