# 模块的导入

### 1.	import  语句

### 2.	from 	...	import	...

### 3.	`__import__（）`内置函数

​		使用内置函数 `__import__()`也可以导入模块：

​		`__m=__import__(name)`	导入模块name到__m

​		内置函数 `__import__()`具有更大的灵活性，例如要导入的模块name可以是计算的结果字符串，但一般不直接使用。

```python
s = 'os'+'.'+'path'
__m = __import__(s)
__m.curdir	
>>>'.'		#即当前文件夹
```

​		事实上，import语句在内部调用该函数。

### 4.	sys.path	模块搜索路径

​		sys模块的sys.path属性返回一个列表，使用import语句导入某一模块时，系统自动从该列表所列的路径中搜索模块，如果没有找到则程序报错。

```python
import sys
sys.path
>>> ['', 'C:\\Users\\muzi_qianjunxiu\\AppData\\Local\\Programs\\Python\\Python37\\python37.zip', 'C:\\Users\\muzi_qianjunxiu\\AppData\\Local\\Programs\\Python\\Python37\\DLLs', 'C:\\Users\\muzi_qianjunxiu\\AppData\\Local\\Programs\\Python\\Python37\\lib', 'C:\\Users\\muzi_qianjunxiu\\AppData\\Local\\Programs\\Python\\Python37', 'C:\\Users\\muzi_qianjunxiu\\AppData\\Roaming\\Python\\Python37\\site-packages', 'C:\\Users\\muzi_qianjunxiu\\AppData\\Local\\Programs\\Python\\Python37\\lib\\site-packages']
```

其中第一个‘ ’ ，表示当前目录。最后一个`'C:\\Users\\muzi_qianjunxiu\\AppData\\Local\\Programs\\Python\\Python37\\lib\\site-packages'`用于扩展模块，建议将自己定义的模块放在这两个位置。

程序中也可以直接修改sys.path列表，以添加模块搜索路径，但这种修改只是临时修改，只适用于包含该代码的程序。

```python
import sys
sys.path.append('c:\\python\works')
```

### 5.	`dir`( )内置函数

​		模块中定义的成员，包括变量函数和类，可通过内置函数dir（ ）查询，列举的成员中包含系统定义的特殊意义的成	员（如 `__xxx__`形式）。 也可通过帮主函数 help（）查询其帮助信息。

​		`dir`（）：不带参数，列举当前模块所有成员

​		`dir`（模块名）：列举指定模块的所有成员

​		`dir`（类/对象）：列举指定类的所有成员

# 模块的定义

### 1.	模块名称

​	每个模块都有一个名称，通过特殊变量 `__name__`获得模块名称。

​	当一个模块被用户单独运行时，其 `__name__`的值为 `__main__`，所以可以把模块源代码文件的测试代码写在相应的测试判断中，以保证只有单独运行时，才会运行测试代码。

```python
if __name__ = '__main__':		#只有独立运行模块代码时，该语句块才会被执行
    pass
```

### 2.	`.pyc 文件`

​		导入模块时，Python解释器为加快程序启动速度，会在与模块文件同一目录下生成 `.pyc` 文件，`.pyc` 文件是经过编译后的字节码，这样下次导入时，如果模块源代码 `.py` 文件没有修改（通过比较两者之间的时间戳），则直接导入 `.pyc` 文件，从而提高程序效率。按字节编译 `.pyc` 文件是在导入模块时python解释器自动完成的，无需程序员手动编译。

# 包

​		python的模块是 `.py` 文件，而包则是文件夹。包可以包含子包，没有层次限制。只要文件夹中包含一个特殊文件 `__init__.py`,则python解释器将该文件夹作为包，其中的模块文件（`.py`文件)则属于包中的模块。特殊文件 `__init__.py`可以为空，也可以包含属于包的代码，当导入包或该包中的模块时，执行 `__init__.py`文件。











