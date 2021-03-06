*Python不是纯函数式编程语言*

# 高阶函数

## 变量与函数名

可以用一个变量指向一个函数名，而代替这个函数的操作。也可以说函数本身可以赋值给变量

```Python
>>> abs(-10)
10
>>> abs
<built-in function abs>
>>> b = abs
>>> b(-29)
29
```

其实函数名本身也是一个变量，是指向函数的变量。下面的代码可以验证这个结论

```Python
>>> abs(-10)
10
>>> def aa(num):
...     print(num + 100)
... 
>>> aa(10)
110
>>> abs = aa
>>> abs(10)
110
>>> abs(-10)
90
>>> 
```

`abs()`是系统提供的求绝对值得函数，我自定义了一个函数`aa()`，让输入的数+100然后打印出来，然后把`aa`函数名赋值给`abs`，然后系统的求绝对值的函数就变成了打印+100操作的函数了。

要恢复`abs()`的函数功能，要重启Python交互环境 `exit()`  -> `python`

---

函数名可以赋值给变量，函数名本身也是个指向函数的变量。函数可以接受变量为参数，那么函数也可以作为参数传给另一个函数。

这句绕口令的意思是：函数可以作为另一个函数的参数，这种函数称为`高阶函数`。

自定义函数`addnum(x,y,z)`包含了三个参数，其中参数`z`是传入的是一个函数名，在`addnum`函数内`z,y`变为函数`z`的参数，执行完函数`z`操作，把结果相加打印出来。

```Python
>>> def addnum(x,y,z):
...     print(z(x) + z(y))
... 
>>> addnum(-10, 10, abs)
20
>>> 
```

## map()

`map()`函数传入两个参数，第一个参数是一个函数，第二个参数是一个可迭代对象`Iterable`，函数将作用于序列的每一个元素，把结果作为`生成器Iterator`返回。

```Python
>>> def st(s):						#自定义函数st
...     return s + a
... 
>>> st('a')							#结果是把传入的字符串拼接一个a'字符
'aa'
>>> a = map(st, '12345')			#调用map函数
>>> a
<map object at 0x101517320>			#返回一个Iterator
>>> list(a)
['1a', '2a', '3a', '4a', '5a']		#转为list
```

## reduce()

`reduce()`函数也接受两个参数，第一个是有两个参数的函数，第二个参数是可迭代对象`Iterable`，两个元素作为参数作用于函数，结果和第三个元素作为参数作用于函数，依次运行下去，最后return结果

```Python
reduce(fun, [x, y, z, m, n]) = fun(fun(fun(fun(x, y) z) m) n)
```

## filter()

`filter()`函数用于筛选过滤序列。操作方式类似`map()`，只是传入的函数返回值是`bool`类型，如果是`True`则保留该元素，如果是`False`则清除该元素，最后剩下的list作为`Iterator`返回

自定义函数，传入整数，偶数返回`True`，奇数返回`False`

```Python
def test(a):
	return a % 2 == 0
```

```Python
>>> list(filter(test, [1,2,3,4,5,6,7]))
[2, 4, 6]    #只保留了偶数
>>>
``` 

## sorted()

`sorted()`是排序算法的高阶函数，可以对list进行排序

```Python
>>> sorted([10,2,3,4,-23,33])
[-23, 2, 3, 4, 10, 33]
>>> 
```

也可以对字符串的list进行排序，是根据ASCII码排序的

```Python
>>> sorted(['a', 'B', 'Za', 'Zb'])
['B', 'Za', 'Zb', 'a']
>>> 
```

还可以接收一个参数`key`函数，来实现自定义排序，比如按照绝对值大小排序

```Python
>>> sorted([10,2,3,4,-23], key = abs)
[2, 3, 4, 10, -23]
>>> 
```

这种实现原理是：key指向的函数作用于list里的每一个元素得到结果，把结果进行排序得到一个新list，命名为：newList，然后将原list按照这个newList里每个元素的对应关系返回相应的元素，实现排序

上文说到，默认的字符串排序是按照ASCII码排序的，大写字母的ASSCII码值要小于小写字母的ASSCII码值，所以`'A'`要排在`'a'`前面。如果要实现按照字母表顺序呢？其实只要忽略每个字符串的大小写就OK

```Python
>>> sorted(['a', 'B', 'Za', 'Zb'], key = str.lower)
['a', 'B', 'Za', 'Zb']
>>> 
```

要实现反向排序，只需要加入第三个参数`reverse = True`

```Python
>>> sorted(['a', 'B', 'Za', 'Zb'], key = str.lower, reverse = True)
['Zb', 'Za', 'B', 'a']
>>> 
```

# 返回函数

## 函数作为返回值

函数既然可以作为参数传递到函数中，自然也可以作为返回值被函数`return`出来。

定义一个求平方的函数

```Python
def test(a):
	return a * a
```

将该函数作为返回值放到另一个函数`testTest()`中

```Python
def testTest(b):
	def test():
		return b * b
	return test
```

调用

```Python
>>> w = testTest(10)
>>> w()
100
>>> 
```

在调用`testTest()`的时候，每次调用都是返回一个全新的函数。即便是传入相同的参数

```Python
>>> w = testTest(10)
>>> z = testTest(10)
>>> w == z
False
>>> 
```

## 闭包

外部函数`testTest()`包含了内部函数`test()`，内部函数`test()`引用了外部函数变量。调用外部函数，在返回函数的时候，包括函数的相关参数都一起返回了。这种程序结构被称为`闭包`。

还有一个注意点，调用外部函数，返回的函数并没有立即执行，只有在调用的时候才会执行。返回的函数不要引用任何可能会变化的变量。

下面一个例子说明返回函数没有立即执行，和返回函数引用会变化的变量引发的问题。

```Python
def  test1():
	array = []
	for x in range(1,5):
		def fs():
			return x
		array.append(fs)
	return array
```

定义函数`test1()`，函数内包含了一个循环，循环的`x`是一个变化的变量，内部函数`fs()`引用了外部这个可变化的变量`x`。每次循环生成一个`fs()`函数，存放到`array`数组中，最后返回`array`数组，里面放了四个`fs`函数。

```Python
>>> f1, f2, f3, f4 = test1()
>>> f1()
4
>>> f2()
4
>>> f3()
4
>>> f4()
4
>>> 
```
最后的结果不是`1`,`2`,`3`,`4`，而全部都是`4`

该函数的运行原理是：调用`test1()`，执行函数内的`for`循环，每一次循环都生成一个`fs()`内部函数，每个内部函数都互不干扰，但是每个函数都引用的外部函数的`x`，最后`for`循环执行完毕，返回了存放函数的数组。这时里面的函数没有任何一个执行了，但是此时的`x`早已变为`4`了。拿到四个函数`f1` `f2` `f3` `f4`运行，所以全部返回`4`。

所以说谨记一点：**返回函数不要引用任何可变化的变量**

# 匿名函数

匿名函数就是没有写函数名的函数。

```Python
list(map(lambda x: x + x, [10,22,4,21]))
[20, 44, 8, 42]
```

这里面`lambda x: x + x`就是匿名函数，`lambda`是表示匿名函数的关键字，`:`前是参数，后表达式。匿名函数只能有一个表达式，不用写`return`，返回值就是该表达式的结果。

匿名函数也是函数，所以也可以赋值给变量，作为参数传入函数或被函数返回。

但是，*Python对匿名函数支持有限，只有一些简单情况下可以用匿名参数*

# 装饰器

不更改函数的情况下，动态增加函数功能的方式，称为`装饰器(Decoratot)`

函数的`__name__`属性可以获得函数名

```Python
>>> test()
helloworld
>>> a = test
>>> a()
helloworld
>>> a.__name__
'test'
>>> 
```

装饰器是一个返回函数的高阶函数。

自定义函数，打印hello world：

```Python
def test():
	print ('hello world')
```
如何在不改动该函数的前提下，打印出调用该函数的函数名？这就使用到了`装饰器`

```Python
def inputTest(func):
	def inputFunc(*args, **kw):
		print('调用函数 %s():' % func.__name__)
		return func(*args, **kw)
	return inputFunc
```
上面代码就是一个打印日志的装饰器，`inputTest`函数，传入了一个函数`func`，返回了函数`inputFunc`。函数内自定义的函数`inputFunc`实现了打印传入函数`func`的函数名，并执行传入函数。

如何将这个装饰器作用于`test`函数？使用Python的`@`语法。

```Python
@inputTest
def test():
	print ('hello world')
```
将`@inputTest`放到`test()`函数定义上面，相当于执行了`test = inputTest(test)`

执行结果：

```Python
>>> test()
调用函数 test():
hello world
>>> 
```

装饰器本身如果需要加入参数，就需要编写一个返回装饰器的高阶函数。例如打印log需要自定义文本+函数名

```Python
def log(text):
	def inputTest(func):
		def inputFunc(*args, **kw):
			print('%s %s():' % (text, func.__name__))
			return func(*args, **kw)
		return inputFunc
	return inputTest

@log('你好')
def test():
	print ('hello world')
```
执行结果：

```Python
>>> test()
你好 test():
hello world
>>> 
```

# 偏函数

Python的`functools`模块提供了很多功能，其中一种就是偏函数(Partial function)。

之前说的函数的[默认参数](https://github.com/mxdios/notebook/blob/master/notebooks/Python%E5%87%BD%E6%95%B0.md#%E9%BB%98%E8%AE%A4%E5%8F%82%E6%95%B0)，指定某一函数的默认值，降低函数调用难度。偏函数也可以做到

函数`int()`可以吧字符串转换为整数，默认按照十进制转换。该函数还有一个额外参数`base`，可以指定按照某进制转换。注：`int('2323232', base = 8)`是指字符串是八进制数据字符串，转为十进制整数

```Python
>>> int('2323232') # 十进制转换十进制
2323232
>>> int('2323232', base = 8) # 八进制转换十进制
632474
>>> int('2323232', base = 16) # 十六进制转换十进制
36844082
>>> 
```

需要大量转换二进制字符串时，每次传入`int(x, base = 2)`太麻烦，自定义函数`int2()`，转换二进制字符串

```Python
>>> def int2(x, base = 2):
...     return int(x, base)
... 
>>> int2('1001001') # 不传base默认按照二进制转换
73
>>> int2('1001001', base = 8)
262657
>>> int2('100110', base = 10)
100110
>>> 
```

上述是基本做法，可以创建偏函数实现上述需求，不需要自定义函数。

```Python
>>> import functools
>>> int2 = functools.partial(int, base = 2)
>>> int2('100111')
39
>>> int2('1001', base = 10)
1001
>>> 
```
`functools.partial`是将函数的某个参数给定一个默认值，返回新的函数。`int2 = functools.partial(int, base = 2)`就是指定`int()`函数中`base`参数的默认值是`2`并返回新的函数。

创建偏函数时，实际上可以接收函数对象、`*args` 和 `**kw` 三个参数。

```Python
>>> max2 = functools.partial(max, 10)
>>> max2(3,9)  # 相当于 max(10, 3, 9)
10
>>> 
```

当函数参数太多时，需要简化，可以使用`functools.partial`创建一个偏函数。可以固定原函数的部分参数，调用起来更简单。


