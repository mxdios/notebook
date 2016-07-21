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

# 生成器

## 介绍

列表生成式只能生成有一定规律的list，比如1-10的偶数平方list，1-100中能被3整除的list。每个元素都是能通过算法计算出来的。

当用到一个list时，为节省存储空间，不用创建出完整的list再去使用，可以在循环中不断推算出后面的元素。类似iOS中的UItableView，只创建显示到屏幕上的cell，在滑动过程中再去创建设置cell(其实是拿内存中闲置的cell，cell的循环机制)。这种边循环边计算的机制称为`生成器(generator)`

## 创建生成器1

创建一个`generator`方法很多，其中一种只要把生成式的`[]`换成`()`即可

```
>>> [x * x for x in range(1,10)]             #列表生成式创建list
[1, 4, 9, 16, 25, 36, 49, 64, 81]
>>> (x * x for x in range(1,10))				#创建generator
<generator object <genexpr> at 0x1025e08e0>  
```

## 遍历生成器元素

生成器是边循环边计算元素的，那怎么打印其中每个元素呢？有一个函数`next()`

```
>>> g = (x for x in range(3))
>>> g
<generator object <genexpr> at 0x1025e0938>
>>> next(g)
0
>>> next(g)
1
>>> next(g)
2
>>> next(g)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
StopIteration
>>>
```

每次调用`next(g)`的时候，计算出下一个元素的值，当超出list的长度时，就会抛出异常`StopIteration(停止迭代)`

这种方法读取元素实在是太low了，generator也是可迭代对象，完全可以用for循环迭代

```
>>> g = (x for x in range(3))
>>> for z in g:
...     print(z)
... 
0
1
2
>>> 
```

## 创建生成器2

有一种数列跟生成器运行机制类似，就是`斐波拉契数列`，除了开始的两个1，每个数都是前两个数的和。这个数列就是边循环边计算得出元素的。

可以用函数打印这样的一个数列，输出指定位数的`斐波拉契数列`

```
def test(num):
	n, a, b = 0, 0, 1
	while n < num:
		print(b)
		a, b = b, a + b
		n = n + 1
```
---
### 插曲:有趣的赋值方式

这里有一种很有趣的赋值方式，在oc和java中看不到。就是上面函数中的`n, a, b = 0, 0, 1`和`a, b = b, a + b`，这是一种同时赋值的方法，`=`右边的值赋值给`=`左边相应位置的参数，每个参数赋值是同时进行的，他们的值都是这条赋值式上面时获得的值。

比如`a, b = b, a + b`,在赋值的时候，`a = b` 和 `b = a + b`是同时进行的，最终的结果都是在`a = 1, b = 2`的前提下得到的。

```
>>> a = 1
>>> b = 2
>>> a, b = b, a + b
>>> a
2
>>> b
3
```

在其他语言中，这种赋值要引入第三变量。不然第二条赋值结果会因为第一条赋值操作而改变。

---
上文中的`test(num)`函数变为生成器很简单，只需要把函数的`print(b)`换成`yield b`

```
def test(num):
	n, a, b = 0, 0, 1
	while n < num:
		yield b
		a, b = b, a + b
		n = n + 1
```

如果一个函数定义中包含了关键字`yield`，那么这个函数不再是一个普通的函数，而是一个生成器

调用结果，返回的是一个generator对象:

```
>>> test(9)
<generator object test at 0x101f0f8e0>
>>> 
```
w
### 生成器的执行原理

在函数中添加了`yield`，这个函数就变成了生成器。这时就改变了Python函数的执行顺序，Python函数是顺序执行的，遇到return或最后一行语句就返回。如果变成了生成器，执行顺序就变成了遇到`yield`返回，下次执行会从上次返回`yield`的位置处继续执行

例如定义函数

```
def test():
	print(1)
	yield 'a'
	print(2)
	yield 'b'
	print(3)
	yield 'c'
	print(4)
	yield 'd'
	return 'END'
```
执行调用：

```
>>> test()
<generator object test at 0x101ae08e0>   #test()函数是生成器generator
>>> a = test()
>>> next(a)     #第一次调用
1
'a'
>>> next(a)     #第二次调用
2
'b'
>>> next(a)     #第三次调用
3
'c'
>>> next(a)     #第四次调用
4
'd'
>>> next(a)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
StopIteration: END
>>> 
```

和第一种创建的生成器一样，用for循环遍历该生成器 test()

```
>>> for n in test():
...     print(n)
... 
1
a
2
b
3
c
4
d
>>> 
```
发现了一个问题，for循环遍历拿不到`return`的返回值`END`。看上面的`next(a)`调用，最后超出范围时抛出的错误` : END`， `END`在这里面。

其实生成器generator的return值在异常错误`StopIteration`的`value`值里

用循环遍历`test()`，并捕获异常，拿到`return`返回的`END`值

```
>>> a = test()
>>> while True:
...     try:
...             x = next(a)
...             print(x)
...     except StopIteration as e:
...             print(e.value)
...             break
... 
1
a
2
b
3
c
4
d
END
>>> 
```

总结：generator是在循环过程中不断计算出下一个元素的值，并在适当条件结束循环。对于函数改为的generator遇到return或者到函数的最后一行语句就是结束generator的指令。就是**函数结束，generator结束**

