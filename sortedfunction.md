# Python3 sorted() 函数

------

## 描述

**sorted()** 函数对所有可迭代的对象进行排序操作。

> **sort 与 sorted 区别：**
>
> sort 是应用在 list 上的方法，sorted 可以对所有可迭代的对象进行排序操作。
>
> list 的 sort 方法返回的是对已经存在的列表进行操作，而内建函数 sorted 方法返回的是一个新的 list，而不是在原来的基础上进行的操作。

## 语法

sorted 语法：

```
sorted(iterable, key=None, reverse=False)  
```

参数说明：

- iterable -- 可迭代对象。
- key -- 指定一个函数，作用在iterator每一个元素上，这个函数只能接收一个参数，按照作用完的iterator元素进行排序。
- reverse -- 排序规则，reverse = True 降序 ， reverse = False 升序（默认）。

## 返回值

返回重新排序的列表。

## 实例

以下实例展示了 sorted 的最简单的使用方法：

```python 
sorted([5, 2, 3, 1, 4]) 
[1, 2, 3, 4, 5]                      # 默认为升序
```

你也可以使用 list 的 list.sort() 方法。这个方法会修改原始的 list（返回值为None）。通常这个方法不如sorted()方便-----如果你不需要原始的 list，list.sort()方法效率会稍微高一些。

```python 
a=[5,2,3,1,4] 
a.sort()  
#[1,2,3,4,5]
```

另一个区别在于list.sort() 方法只为 list 定义。而 sorted() 函数可以接收任何的 iterable。

```python 
sorted({1: 'D', 2: 'B', 3: 'B', 4: 'E', 5: 'A'}) 
#[1, 2, 3, 4, 5]
```

利用key进行倒序排序

```python 
example_list = [5, 0, 6, 1, 2, 7, 3, 4]
result_list = sorted(example_list, key=lambda x: x*-1)
print(result_list)
#[7, 6, 5, 4, 3, 2, 1, 0]
```

要进行反向排序，也通过传入第三个参数 reverse=True：

```python
example_list = [5, 0, 6, 1, 2, 7, 3, 4] 
sorted(example_list, reverse=True) 
#[7, 6, 5, 4, 3, 2, 1, 0]
```

