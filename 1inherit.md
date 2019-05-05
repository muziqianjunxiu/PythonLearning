# 继承

python支持多重继承，即一个派生类可以继承多个基类。如果在类定义中没有指定基类，则默认基类是object，object是所有对象的根基类，定义了公用方法的默认实现，如`__new__`方法。多个类的继承可以形成层次关系，通过类的方法mro（）或类的属性 `__mro__` 可以输出其继承的层次关系。

## `__init__`函数的继承

声明派生类时，必须在其构造函数中调用基类的构造函数。即继承构造函数。

如果我们要给实例 c 传参，我们就要使用到构造函数，那么构造函数该如何继承，同时子类中又如何定义自己的属性？

继承类的构造方法：

​        1.经典类的写法： 父类名称.__init__(self,参数1，参数2，...)

​        \2. 新式类的写法：super(子类，self).__init__(参数1，参数2，....)

```python
`class` `Person(``object``):` `    ``def` `__init__(``self``, name, age):``        ``self``.name ``=` `name``        ``self``.age ``=` `age``        ``self``.weight ``=` `'weight'` `    ``def` `talk(``self``):``        ``print``(``"person is talking...."``)`  `class` `Chinese(Person):` `    ``def` `__init__(``self``, name, age, language):  ``# 先继承，在重构``        ``Person.__init__(``self``, name, age)  ``#继承父类的构造方法，也可以写成：super(Chinese,self).__init__(name,age)``        ``self``.language ``=` `language    ``# 定义类的本身属性` `    ``def` `walk(``self``):``        ``print``(``'is walking...'``)`  `class` `American(Person):``    ``pass` `c ``=` `Chinese(``'bigberg'``, ``22``, ``'Chinese'``)`
```

　　如果我们只是简单的在子类Chinese中定义一个构造函数，其实就是在重构。这样子类就不能继承父类的属性了。所以我们在定义子类的构造函数时，要先继承再构造，这样我们也能获取父类的属性了。

## 重写

通过继承，派生类继承基类中除构造方法之外的所有成员。如果在派生类中重新定义从基类继承的方法，则派生类中定义的方法覆盖从基类继承的方法。