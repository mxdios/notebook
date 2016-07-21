# 切片

切片就是切割list或者tuple里面的元素，获取某个或某段元素。

```
>>> L = ['a', 'b', 'c', 'd', 'e']
>>> L[:1]
['a']
>>> L[:3]
['a', 'b', 'c']
>>> L[2:3]
['c']
>>> 
```

`L[a:b]` 表示返回从第`a`个元素到第`b`个元素，不包含第`b`个元素。 如果索引为`0`的话可以省略。

同样支持倒数切片，倒数第一个元素是的索引是`-1`

```
>>> L[-2:-1]
['d']
>>> L[-1:]
['e']
>>> L[-4:-1]
['b', 'c', 'd']
>>> 
```

切片操作不仅可以切割相邻的元素，还可以分段切割

```
>>> l = [1,2,3,4,5,6,7,8,9,10]
>>> l[::2]
[1, 3, 5, 7, 9]
>>> l[1:3:2]
[2]
>>>
``` 

`L[a:b:c]` 表示返回从第`a`个元素到第`b`个元素(不包含第`b`个元素),每`c`个元素取一个。

同样对tuple也支持，只是返回的结果格式还是tuple

对字符串也同样适用，返回的还是字符串

```
>>> str = '123456789'
>>> str[1:4]
'234'
>>> str[::3]
'147'
>>> 
```

切片是一种很高能的操作。能随意截取list、tuple甚至字符串，相比于oc截取字符串的某一段，Python这种切片操作简单极了

# 迭代

用`for`循环来遍历一个可遍历对象称为迭代

可遍历对象: list、 tuple、 dict、 set、str...

## 迭代list

```
>>> l = [1, 2, 3]
>>> for a in l:
...     print(a)
... 
1
2
3
>>> 
```

## 迭代tuple

```
>>> tup = (1,2,3)
>>> for a in tup:
...     print(a)
... 
1
2
3
>>>
```

## 迭代dict

dict是key-value存储的，默认迭代的是key， 若想迭代value需要`.values()`, 若想同事迭代key-value 需要`.items()`。

dict的存储顺序是无序的，迭代结果的顺序并不一定按照顺序排列

```
>>> dict = {1:'a', 2:'b', 3:'c'}
>>> for d in dict:					#默认迭代key
...     print(d)
... 
1
2
3
>>> for d in dict.values():       #迭代value
...     print(d)
... 
a
b
c
>>> 
>>> for k, v in dict.items():		#迭代key-value
...     print(k, v)
... 
1 a
2 b
3 c
>>> 
```

## 迭代set

```
>>> sets = set([1,2,3,4,4,2])
>>> sets
{1, 2, 3, 4}
>>> for k in sets:
...     print(k)
... 
1
2
3
4
>>> 
```

## 迭代str

```
>>> str = 'abc'
>>> for s in str:
...     print(s)
... 
a
b
c
>>> 
```

## 带索引的list迭代

迭代一个list，还需要显示list的索引。在Java或oc中有`for(int i = 0; i < list.count; i ++)`这样的for循环，打印i就是list的索引。 在Python中有`enumerate()`函数，能拿到list的索引

```
>>> l = ['a','b','c']
>>> for i, s in enumerate(l):
...     print(i, s)
... 
0 a
1 b
2 c
>>> 
```

## 多个迭代变量

迭代时两个变量，比如一个list里面放了tuple `[(1,2),(3,4),(5,6)]` 可以引入两个迭代变量，遍历内容。 注意点是tuple的元素个数必须固定，跟给定的迭代变量参数个数相同，否则会报错

```
for a, b in [(1,2),(3,4),(5,6)]
	print(a, b)
# 结果
1 2
3 4
5 6
```

## 判断对象是否可迭代

引入`collections`模块的`Iterable`做类型判断

```
>>> from collections import Iterable
>>> isinstance(10, Iterable)   #整数不可迭代
False
>>> isinstance(10.4, Iterable)   #浮点型不可迭代
False
>>> isinstance([10,11], Iterable)  #list可以迭代
True
>>> isinstance({10:'a',11:'b'}, Iterable)   #dict可以迭代
True
>>> 
>>> isinstance({10,11}, Iterable)   #set可迭代
True
```

# 列表生成式

`List Comprehensions`列表生成式，用简单的代码生成复杂的list

生成`[1,2,3,4]` 使用`list(range(1,5))`，x从`1`到`5`不包括`5`

```
>>> list(range(1,5))
[1, 2, 3, 4]
>>> 
```
## 普通列表生成式

如果要生成`[1*1, 2*2, 3*3, 4*4, 5*5]`的list的话怎么生成呢？ 可以用for循环

```
>>> l = []
>>> for x in range(1,5):
...     l.append(x * x)
... 
>>> l
[1, 4, 9, 16]
```

如果要用列表生成式的话，只需要一行代码就能搞定

```
>>> [x * x for x in range(1,5)]
[1, 4, 9, 16]
>>>
``` 

列表生成式`[x * x for x in range(1,5)]`可以这样理解：`for`循环是生成元素的，`for`循环生成的元素放到前面结果元素计算式`x * x`中计算得到最终结果，结果本身就在`[]`中，不需要`append`了。

## 带筛选条件的列表生成式

列表生成式还可以增加筛选条，比如，生成1-10(包括10)中所有偶数的平方组成的list

```
>>> [x * x for x in range(1,11) if x % 2 == 0]
[4, 16, 36, 64, 100]
```

筛选条件跟在for循环后面，筛选for循环生成的元素，让只有符合条件的元素进入`结果元素计算式`中计算，组成最终的list

## 两层循环的列表生成式

需求：列出两个list里元素的两两一起的所有组合

```
>>> [n + m for n in ['a','b','c','d'] for m in ['A','B','C','D']]
['aA', 'aB', 'aC', 'aD', 'bA', 'bB', 'bC', 'bD', 'cA', 'cB', 'cC', 'cD', 'dA', 'dB', 'dC', 'dD']
>>>
``` 
根据打印结果知道，其实就是循环嵌套。这是二层嵌套，其实也可以有n层嵌套，可以试试。下面是三次for循环，可以自己试试

```
>>> [n + m + x for n in ['a','b','c','d'] for m in ['A','B','C','D'] for x in '1234']
```

## 多个变量的列表生成式

嵌套循环其实就是多个变量的列表生成式，一层循环的多个变量同样适用。比如迭代(遍历)dict，同时迭代出key-value，然后组成list

```
>>> [str(k) + v for k,v in {1:'a', 2:'b', 3:'c', 4:'d'}.items()]   #str()是类型转换，迭代出来的k是int类型，要想与字符串拼接要先转成str类型
['1a', '2b', '3c', '4d']
>>> 
```

