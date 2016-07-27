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

外部函数`testTest()`包含了内部函数`test()`，内部函数`test()`引用了外部函数变量。调用外部函数，在返回函数的时候，包括函数的相关参数都一起返回了。这种程序结构被称为`闭包`。

在调用`testTest()`的时候，每次调用都是返回一个全新的函数。

```Python
>>> w = testTest(10)
>>> z = testTest(10)
>>> w == z
False
>>> 
```
还有一个注意点，调用外部函数，返回的函数并没有立即执行，只有在调用的时候才会执行。

## 闭包

