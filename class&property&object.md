# 属性

## 实例属性

实例属性一般在`__init__`方法中初始化定义，通过`self.`进行访问。

## 类属性

Python也允许声明属于类本身的变量，即类属性。类属性属于整个类，不是特定实例的一部分，而是所有实例之间共享一个副本。在其类定义的方法中或外部代码中，通过类名访问：

`类名.类变量名=值`			#写入

`类名.类变量名`				#读取

```python
class Person:
    count = 0				#定义类属性count，表示计数
    def __init__(self,name):
        slef.name = name
        Person.count += 1	#创建一个实例时，计数加一
    def __del__(slef):		#析构函数
        Person.count -= 1	#销毁一个实例时，计数减一
    def get_count():		#创建类方法
        print("实例个数为",Person.count)
p1 = Person('zhang')		#创建对象
Person.count				#类名访问
p2 = Person('li')			#第二个对象
Person.get_count()			#通过类方法访问
del p1
Person.count
>>> 1
	实例个数为2
    1
```

## 私有属性

Python类的成员没有访问控制限制，这与其他编程语言不同。

通常，约定**两个下划线开头，但不以两个下划线结尾的属性是私有的**，其他的为公共的。

不能直接访问私有属性，但是可以在方法中访问。

```python
class A:
    __name = 'wang'		#私有类属性
    def get_name(self):
        print(A.__name)	#在类方法中访问私有类属性
>>>A.get_name()			#wang
>>>A.__name				#AttributeError:type object ‘A’ has no attribute '__name'
```

## @property装饰器

面向对象编程的封装性原则要求不直接访问类中的数据成员。Python中可以通过定义私有属性，然后定义相应的访问该私有属性的函数，并使用@property装饰器装饰这些函数。从而，程序可以将函数“当做”属性访问，从而提供更加可控且友好的访问方式。

@property装饰器定义**只读**属性，@setter定义**可读可写**属性，@deleter定义**可读可写可删除**属性

简单来说，@property的作用其实就是将方法变成属性进行调用。

```python
class Person:
    def __init__(self,name):
        self.__name = name
    @property
    def name(self):
        '''name property doc'''
        return self.__name
    @name.setter
    def name(self,val):
        self.__name = val
    @name.deleter
    def name(self):
        del self.__name 
if __name__ == '__main__':
    p=Person('wang')
    print(p.name)
    p.name = 'xuan'
    print(p.name)
>>> wang
	xuan
```

另一种property的调用格式为：

**property(fget = None , fset = None , fdel = None , doc = None)**

其中，fget为get访问器，fset为set访问器，fdel为del访问器

```python
class Person:
    def __init__(self,name):
        self.__name = name
    def getname(self):
        return self.__name
    def setname(self,val):
        self.__name = val
    def delname(self):
        del self.__name 
    name = property(getname,setname,delname,'name property doc')	#这样操作后，name就成了Person的属性而非方法了。
if __name__ == '__main__':
    p=Person('wang');print(p.name)
    p.name = 'xuan';print(p.name)
>>> wang
	xuan
```

## 特殊属性

Python对象中包含许多以双下划线开始是和结尾的方法，称为特殊属性。

| 特殊方法                          | 含义                     |
| --------------------------------- | ------------------------ |
| `object.__dict__`                 | 对象的属性字典           |
| `instance.__class__`              | 对象所属的类             |
| `class.__bases__、class.__base__` | 类的基类元组    类的基类 |
| `class.__name__`                  | 类的名称                 |
| `class.__qualname__`              | 类的限定名称             |
| `class.__mro__`                   | 方法查找顺序，基类元组   |
| `class.mro()`                     | 同上，可被子类重写       |
| `class.__subclass__`              | 子类列表                 |

## 自定义属性

### `object.__getattr__(self, name)`

实例`instance`通过`instance.name`访问属性`name`，只有当属性`name`没有在实例的`__dict__`或它构造类的`__dict__`或基类的`__dict__`中没有找到，才会调用`__getattr__`。当属性`name`可以通过正常机制追溯到时，`__getattr__`是不会被调用的。如果在`__getattr__(self, attr)`存在通过`self.attr`访问属性，会出现无限递归错误。

```python
class ClassA(object):

    def __init__(self, classname):
        self.classname = classname

    def __getattr__(self, attr):
        return('invoke __getattr__', attr)

insA = ClassA('ClassA')
print(insA.__dict__) # 实例insA已经有classname属性了
# {'classname': 'ClassA'}

print(insA.classname) # 不会调用__getattr__
# ClassA

print(insA.grade) # grade属性没有找到，调用__getattr__
# ('invoke __getattr__', 'grade')
```

### `object.__getattribute__(self, name)`  

实例`instance`通过`instance.name`访问属性`name`，`__getattribute__`方法一直会被调用，无论属性`name`是否追溯到。如果类还定义了`__getattr__`方法，除非通过`__getattribute__`显式的调用它，或者`__getattribute__`方法出现`AttributeError`错误，否则`__getattr__`方法不会被调用了。如果在`__getattribute__(self, attr)`方法下存在通过`self.attr`访问属性，会出现无限递归错误。  如下所示，`ClassA`中定义了`__getattribute__`方法，实例`insA`获取属性时，都会调用`__getattribute__`返回结果，即使是访问`__dict__`属性。**

```python
class ClassA(object):

    def __init__(self, classname):
        self.classname = classname

    def __getattr__(self, attr):
        return('invoke __getattr__', attr)

    def __getattribute__(self, attr):
        return('invoke __getattribute__', attr)


insA = ClassA('ClassA')
print(insA.__dict__)
# ('invoke __getattribute__', '__dict__')

print(insA.classname)
# ('invoke __getattribute__', 'classname')

print(insA.grade)
# ('invoke __getattribute__', 'grade')
```

### `object.__setattr__(self, name, value)` 

 如果类自定义了`__setattr__`方法，当通过实例获取属性尝试赋值时，就会调用`__setattr__`。  常规的对实例属性赋值，被赋值的属性和值会存入实例属性字典`__dict__`中。

```python
class ClassA(object):

    def __init__(self, classname):
        self.classname = classname

insA = ClassA('ClassA')

print(insA.__dict__)
# {'classname': 'ClassA'}

insA.tag = 'insA'    

print(insA.__dict__)
# {'tag': 'insA', 'classname': 'ClassA'}
```

如下类自定义了`__setattr__`,对实例属性的赋值就会调用它。类定义中的`self.attr`也同样，所以在`__setattr__`下还有`self.attr`的赋值操作就会出现无线递归的调用`__setattr__`的情况。自己实现`__setattr__`有很大风险，一般情况都还是继承`object`类的`__setattr__`方法。

```python
class ClassA(object):
    def __init__(self, classname):
        self.classname = classname

    def __setattr__(self, name, value):
        # self.name = value  # 如果还这样调用会出现无限递归的情况
        print('invoke __setattr__')

insA = ClassA('ClassA') # __init__中的self.classname调用__setattr__。
# invoke __setattr__

print(insA.__dict__)
# {}

insA.tag = 'insA'    
# invoke __setattr__

print(insA.__dict__)
# {}
```

### `object.__delattr__(self, name)`

```
Like __setattr__() but for attribute deletion instead of assignment. This should only be implemented if del obj.name is meaningful for the object.
```

### `object.__dir__(self)  dir()`

作用在一个实例对象上时，`__dir__`会被调用。返回值必须是序列。dir()将返回的序列转换成列表并排序。

### `object.__call__(self[, args...])`

```python
Called when the instance is “called” as a function; if this method is defined, x(arg1, arg2, ...) is a shorthand for x.__call__(arg1, arg2, ...).
```

Python中有一个有趣的语法，只要定义类型的时候，实现`__call__`函数，这个类型就成为可调用的。换句话说，我们可以把这个类的对象当作函数来使用，相当于重载了括号运算符。

```python
class Student(object):
    def __init__(self, name):
        self.name = name
    def __call__(self):
        print('My name is %s.' % self.name)
        
s = Student('Michael')
s()
# My name is Michael.
```

通过使用`__setattr__`, `__getattr__`, `__delattr__`可以重写dict,使之通过“.”调用键值。

```python
class Dict(dict):
    '''
    通过使用__setattr__,__getattr__,__delattr__
    可以重写dict,使之通过“.”调用
    '''
    def __setattr__(self, key, value):
        print("In '__setattr__")
        self[key] = value
        
    def __getattr__(self, key):
        try:
            print("In '__getattr__")
            return self[key]
        except KeyError as k:
            return None
            
    def __delattr__(self, key):
        try:
            del self[key]
        except KeyError as k:
            return None
            
    # __call__方法用于实例自身的调用,达到()调用的效果
    def __call__(self, key):    # 带参数key的__call__方法
        try:
            print("In '__call__'")
            return self[key]
        except KeyError as k:
            return "In '__call__' error"
            
s = Dict()
print(s.__dict__)
# {}

s.name = "hello"    # 调用__setattr__
# In '__setattr__

print(s.__dict__) # 由于调用的'__setattr__', name属性没有加入实例属性字典中。
# {}

print(s("name"))    # 调用__call__
# In '__call__'
# hello

print(s["name"])    # dict默认行为
# hello

# print(s)
print(s.name)       # 调用__getattr__
# In '__getattr__
# hello

del s.name          # 调用__delattr__
print(s("name"))    # 调用__call__
# None
```













































































