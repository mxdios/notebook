# 函数

## 一些函数举例

- 绝对值 abs(-1)

- 最大值 max(1,2,3)

- 类型转换 int('123')  float('12.3') str(100) bool(1)

- 整数转十六进制 hex(10)

给函数起别名

```Python
>>> a = abs
>>> a(-1)
```
## 自定义函数

在Python交互环境中自定义函数

```Python
>>> def myAbs(x):       #def是定义函数的关键字 myAbs是函数名 x是参数
...     if x >= 0:      #注意要缩进四个空格，不然会自定义失败
...          return x   #同样要缩进，if判断语句的写法，代码块
...     else:
...          return -x
... 					 #两个回车，退出函数编辑
>>> myAbs(-5)			 #调用函数
5
>>> myAbs(5)
5
>>> 
```

在.py文件中自定义函数并运行

```Python
def my_abs(x):
if x >= 0:
	return x
else:
	return -x

print(my_abs(10))		#调用函数，并打印出结果
print(my_abs(-10))
```

将函数封装在一个.py文件，并在Python交互环境中调用

将下面函数代码写入一个.py文件，并将该文件命名为myabs.py

```Python
def my_abs(x):
if x >= 0:
	return x
else:
	return -x
```

python 进入Python交互环境

```Python
>>> from myabs import my_abs  #引入函数  myabs是文件名不带.py， my_abs是文件里的函数名
>>> my_abs(-5)     			   #调用函数
5
```

## 空函数与pass语句

自定义一个空函数

```Python
def nodef():
    pass
```

pass 就是什么都不做，用作占位符，能让程序运行起来，不报错。没有它会有语法错误

用Sublime text文本编辑器写好多语句时，都会自动加上pass

## 参数检查

自定义的my_abs()函数相比于系统的abs函数还是不一样。比如传入错误类型的参数'a'

两者报错不同。系统abs函数会检查出参数错误，而自定义的my_abs 是if判断条件错误。所以要增加参数类型检查，判断如果是不符合的参数类型，抛出异常

检查数据类型用isinstance()函数

```Python
def my_abss(x):
if not isinstance(x, (int, float)):   ##如果不是int和float类型的话，抛出类型异常
	raise TypeError('输入类型错误')
if x >= 0:
	return x
else:
	return -x
```

**写这个的时候遇到了一个问题，一个看代码很难发现的问题**

错误提示如下：

```
TabError: inconsistent use of tabs and spaces in indentation
#翻译: 在缩进制表符和空格的使用不一致
```

在我进行缩进的时候，不同位置用了不同的缩进方式，四个空格和tab共用了，然后就报错了。我尝试了一下，全部使用四个空格缩进和全部使用tab缩进都可以，但是不能混合使用。

## 函数多个返回值

函数可以有多个返回值，return 时用分号分割开来每个返回值即可

```Python
def my_abss(x):
	if not isinstance(x, (int, float)):
		raise TypeError('输入类型错误')
	if x >= 0:
		return x, x + 1
	else:
		return -x, x + 1
```

返回结果 

```Python
>>> my_abss(10)
(10, 11)
```
	
其实返回的就是一个tuple

## 小结

- 定义函数需要写函数名和参数

- 做必要的参数数据类型检查

- 随时随地都可以return结果

- 可以有多个返回值，以tuple的形式返回

- 函数执行完毕也没有return，会return None


# 函数参数

## 位置参数

计算某一个数的平方值

```Python
def myPower(a):
	return a * a
	
#参数a就是一个位置参数
```

另一种需求，我要计算 a的3次方、4次方、5次方…… 可以加一个参数，把函数改为

```Python
def myPower(a, n):
	s = 1
	while n > 0:
		n = n - 1
		s = s * a
	return s
```

这样就可以完成需求了。

但是，算平方是最常用的，在算平方的时候依旧输入两个参数 myPower(2, 2)，这样感觉很费劲。

Python中有一个叫默认参数的东西可以解决这个问题

## 默认参数

指定某一个参数的默认值，如果这个参数不传值的话就用某个值。

```Python
def myPower(a, n = 2):  #如果n没有传值，则n=2
	s = 1
	while n > 0:
		n = n - 1
		s = s * a
	return s
```

调用函数

```Python
>>> myPower(2, 1)
2
>>> myPower(2)
4
>>> 
```
	
**注意点：有多个参数时，必须按参数必须在前面，默认参数在后面**

因为默认参数如果在必选参数前面的话，调用时输入参数，第一个参数无法确定是给默认参数的还是给必选参数的。

但是，只有一个参数的时候，可以把这个参数设置为默认参数。

---

*有两个默认参数的时候怎么办？*

```Python
def classname(name, age, city = 6, gender = 'Beijing'):
	print(name, age, city, gender)
```

调用结果:
	
```Python
>>> classname('小明', 10)
小明 10 6 Beijing
>>> classname('小明', 10, 10, 'tianjing')
小明 10 10 tianjing
>>> 
>>> classname('小明', 10, 15)
小明 10 15 Beijing
>>> classname('小明', 10, 'shanghai')
小明 10 shanghai Beijing
>>> 
```

前三次调用都没有问题，最后一次有问题了。

有多个默认参数的时候，调用时按顺序写入参数没有问题，如果要跳过某个默认参数输入后面的默认参数，就要指定参数名

```Python
>>> classname('小明', 10, gender = 'shanghai')
小明 10 6 shanghai
>>> 
```

---

默认参数还有一个注意点: **默认参数必须为指向不可变的对象**

举例：以list为默认参数，设置函数。 给定一个list，添加'z'元素返回lsit。默认list为['a']

```Python
def addend(L = ['a']):
	L.append('z')
	return L
```

调用结果为

```Python
>>> addend()
['a', 'z']
>>> addend()
['a', 'z', 'z']
>>> addend()
['a', 'z', 'z', 'z']
>>> addend()
['a', 'z', 'z', 'z', 'z']
>>> addend()
['a', 'z', 'z', 'z', 'z', 'z']
>>> 
```

每次运行函数记住了上次的结果，叠加了内容

因为参数L 是一个变量，指向一个对象list ['a'] , list是可变的，每次运行L指向的list都改变了，不再是默认参数['a']

所以 **默认参数必须指向不可变对象！！**

其实上述需求也是可以实现的

```Python
def addend(L = None):
if L is None:
	L = ['a']
L.append('z')
return L
```

调用结果

```Python
>>> addend()
['a', 'z']
>>> addend()
['a', 'z']
>>> addend()
['a', 'z']
>>> 
```

这种实现方式类似于oc中在方法内部处理某个参数的操作，如果某个参数传了nil，则参数初始化为某个值

## 可变参数

计算多个数字的平方和，用list或者tuple包一组数，传入函数

```Python
def calc(num):
	sum = 0
	for n in num:
		sum = sum + n * n
	return sum
```

调用结果

```Python
>>> calc([1,2,3])
14
>>> 
```

---

可以用可变参数，实现并不需要用list或tuple包裹数字，直接传入数 calc(1,2,3)。 可变参数会在函数调用时自动组装为一个tuple

代码实现：

```Python
def calc(*num):			#加一个*即可
	sum = 0
	for n in num:
		sum = sum + n * n
	return sum
```
	
调用结果

```Python
>>> calc(1,2,3)
14
>>> 
```

**可变参数可以传入任意个参数包括0个。cals()也可以**

**可变参数一般要放到所有参数的最后面，如果可变参数后面还跟着其他参数，这些参数有独特的含义——命名关键字参数**

---

这时如果有一组list或者tuple数据要计算平方和，nums = [1,2,3], 单个元素取出传入calc函数 calc(nums[0], nums[1], nums[2])，这样做肯定是可以的，但是太麻烦，如果list有一百个元素就疯了。如果把整个list传入的话又会报错

```Python
>>> calc([1,2,3])
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "/Users/inspiry/Desktop/myabs.py", line 7, in calc
    sum = sum + n * n
TypeError: can't multiply sequence by non-int of type 'list'
>>> 
```

简单，传入list或者tuple的时候加一个*，

Python允许添加*号， 把list或tuple变为可变参数传入函数中

```Python
>>> calc(*[1,2,3])
14
```

**默认参数和可变参数的概念swift中也有，应该是借鉴Python吧**

## 关键字参数

关键字参数是用两个星号(**)表示的，会在函数调用时自动组装为dict

```Python
def calc(name, age, **other):
	print('name:', name, 'age:', age, 'other:', other)
```

**other为关键字参数，可以传入任意个参数，也可以不传入。传入参数时是以key-value的方式传的

调用:

```Python
>>> calc('mxd', 10)
name: mxd age: 10 other: {}
>>> calc('mxd', 18, a = 'a', b = 20)
name: mxd age: 18 other: {'a': 'a', 'b': 20}
```

类比可变参数可以把list添加一个*传入函数中作为参数，同样关键字参数也可以这样做。把一个dict 传入函数当做关键字参数。
	
```Python
>>> oth = {'job': 'ios', 'book': 'macbook pro'}
>>> calc('mxd', 18, **oth)
name: mxd age: 18 other: {'job': 'ios', 'book': 'macbook pro'}
```

注意点: 函数 calc(name, age, **other) 中的other得到了一个oth指向的dict，得到的其实是oth的一份拷贝，在函数内对其改变操作，不会影响外面的oth

## 命名关键字参数

命名关键字参数是给参数起名，关键字参数虽然以key-value的方式传入参数，给每个参数制定了key，但是这个key还是不能确定，调用者可以传入不受限制的关键字参数。

限制关键字参数的key，就是指定关键字参数的名字，就用到了命名关键字参数

```Python
def calc(name, *, age):
	print(name, age)
```

命名关键字参数的写法是以*作为分隔符，分号后面的参数为命名关键字参数。可以跟多个参数，def calc(name, *, age, other):  这时age 和 other都是命名关键字

调用时必须传入，且必须以 命名 = 内容 的key-value方式调用

```Python
>>> calc('mxd', age = 18)
mxd 18
>>> 
```
	
---

之前说可变参数要放到参数最后面，后面再跟参数的话就是命名关键字参数了。

```Python
def calc(name, *age, other, zz):
	print(name, age, other, zz)
```

other 和 zz 都是命名关键字了，必须传入，调用:

```Python
>>> calc('mxd', 18, 11, other = '2', zz = 3)
mxd (18, 11) 2 3
```

---

命名关键字参数也可以指定参数默认值，指定other参数的默认值为'good'

```Python
def calc(name, *age, other = 'good', zz):
	print(name, age, other, zz)
	
```

调用

```Python
>>> calc('mxd', 32,22,22, zz = 'a')
mxd (32, 22, 22) good a
>>> calc('mxd', 10, other = 'no', zz = 'b')
mxd (10,) no b
>>> 

```

## 参数组合

**5种参数：必选参数、默认参数、可变参数、关键字参数、命名关键字参数。在使用的时候如果有多种参数共同存在是，顺序必须为：必选参数、默认参数、可变参数、命名关键字参数、关键字参数**