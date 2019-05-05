# 对象的引用

对象的赋值实际上是对象引用，创建一个对象并把它赋值给多个变量时，这些变量是指向该对象的引用，其id（）返回值都是一样的，即原对象的id值。

```python
a=[1,2,3]
b=a
id(a)==id(b)		#True
```

# 对象的浅拷贝

对象的赋值引用同一个对象，即**不拷贝**对象。如果要拷贝对象，（即所得对象的id值是新的）可使用下列方法：

- 切片操作。如：a[:]

- 对象实例化。如：list(a)

- copy模块的copy函数。如：copy.copy(a)

  但需注意，copy.copy（）是浅拷贝，即拷贝对象时，并不拷贝对象中包含的子对象，而是引用被拷贝对象的子对象（此时子对象的id值还是原对象中子对象的id值，但浅层的新对象id值已经改变了。）。所以在对该对象拷贝所得的子对象进行操作时，原对象中的子对象也会随之改变。

# 对象的深拷贝

如果要递归拷贝对象中包含的子对象，可使用copy.deepcopy（）函数。此时获得的对象的id值完全是新的。
