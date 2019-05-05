# operator模块和运算符函数

operator模块提供了一系列函数，这些函数封装了运算符的功能。将运算符封装为函数对象，可以作为参数传递给需要函数对象作为参数的场合，实现灵活编程的目的。

## eval、exec和compile函数



### 1.运行上下文的局部变量和全局变量

程序运行期间在上下文中会生成各种局部变量和全局变量。使用内置函数globals（）和locals（），可以返回他们的字典映射表。

无参内置函数vars（）等同于locals（），有参内置函数vars（object）返回指定对象的字典列表，等同于object.__dict__。



### 2.eval函数和动态表达式的求值

使用内置的eval函数可以对动态表达式进行求值：

**eval(expression,globals=None,locals=None)**

其中，expression是动态表达式的字符串；globals和locals是求值时使用的上下文环境的全局变量和局部变量，如果不指定，则使用当前上下文

```python
x=2

str_func=input(‘请输入表达式’)

>>>请输入表达式：x**2+2*x+1

eval(str_func)	

>>>9
```

eval函数的功能是将字符串生成语句执行，如果字符串包含不安全的语句，则存在注入安全隐患，如删除文件的语句。



### 3.exec函数和动态语句的执行

使用内置的exec函数可以执行动态语句：

**exec(str  [, globals  [, locals]])**

其中str是动态语句的字符串；globals和locals是使用的上下文环境的全局和局部变量，如果不指定则使用当前上下文。

通常eval用于动态表达式求值，返回一个值；exec用于动态语句的执行，不返回值。exec也同样存在注入安全隐患。

```python
exec(“for i in range(10) : print(i)” )  	
>>>0 1 2 3 4 5 6 7 8 9
```



### 4.compile函数和动态语句的执行

使用内置的compile函数可以编译代码为代码对象：

**compile( source,filename,mode)	#返回代码对象**

其中，source为代码语句的字符串，如果是多行语句则每一行的结尾必须有换行符\n。filename为包含代码的文件。mode为编译模式，可以为：‘exec’用于语句系列执行，‘eval’用于表达式求值，‘single’用于单个交互语句。

编译后的代码对象可以通过eval函数或exec函数执行，因为编译为代码对象，所以可以提高效率。

```python
co=compile ( “for i in range(10) : print(i)”, ‘’, ‘exec’)

exec(co)		

>>>0 1 2 3 4 5 6 7 8 9
```

