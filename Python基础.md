# 安装Python环境

- 在安装了Homebrew的前提下，终端命令brew install python3

- 安装Homebrew
	
		/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
		
- 安装完成之后输入 python  终端显示>>>，  说明Python安装成功，进入了Python交互环境，输入Python代码回车得到执行结果

- 退出Python交互环境  exit() 回车。 或者直接关闭终端


# 第一个Python程序

在终端环境下，输入python 进入>>>  Python交互环境

- 打印hello world

		print("hello world")
		或者
		print('hello world')

- 数字计算，直接输入 100+33 回车 会得到133的结果，任何有效的数学计算都能算出来

运行.py文件 (包含Python的代码是以.py为拓展名的文本文件，需要用Python解析器去执行该文件)

- 终端中输入 python 文件名.py 回车，执行.py文件中的Python代码

**注意点: 文本文件中的Python代码必须是执行代码，例如打印100+10，不能像终端>>>Python交互环境中100+10这样写，而应该写为print(100+10),这样才能输出结果**

# Python代码文本执行

## Python交互环境下执行代码文件

- 创建一个文本文件hello.py, 文本文件里写print("hello world")。 终端cd切换到文件所在的目录。执行代码，终端会打印出hello world

		python hello.py

## 直接运行.py文件，执行代码文件

- 在终端中直接运行.py文件，不用写python。在Windows环境下不可以，在mac和Linux下是可以的。

- 更改文本文件hello.py里的代码
	
		#!/usr/bin/env python3
		
		print("hello world")

- 在终端中赋予hello.py执行权限
		
		chmod a+x hello.py

- 直接运行hello.py
		
		./hello.py
		
# Python代码助手

python代码助手是在先输入Python代码，然后本机运行Python脚本执行代码。

脚本地址 [learning.py](https://raw.githubusercontent.com/michaelliao/learn-python3/master/teach/learning.py) 右键保存

在终端中 python learning.py 运行。显示 **Ready for Python code on port 39093...** 运行成功

问题： 提醒 **Your current python is 2.7. Please use Python 3.4.**  Mac上自带的Python版本是2.7，装了3.4，两个版本并存了，需要把2.7卸载掉，过或者把3.4放到path的前面。或者在输入执行脚本 ： python3 learning.py

可以使用 pyenv 或者 virtualenv 切换Python版本


# 输入输出

## 输出

- 输出字符串

		print('hello world') 或者 print('hello', 'world')  
		#结果 hello world

- 输出计算结果
	
		print(10+10)  #20
		
- 输出字符串+计算结果

		print('10+10=', 10+10) #10+10= 20
		
## 输入

- 输入操作


		name = input()
		mxd
		print(name)   #mxd

- 有内容提醒的输入操作

		name = input('请输入名字')
		mxd
		print('你好' + name) #你好 mxd
		
# Python的语法格式

		# 是注释  
		
- 每一行都是一个语句，不用加; 结尾。 当以:结尾时，第二行缩进为代码块，例如 if 语句

		a = 1
		if a > 10:
    		print(a)
		else:
    		print(-a)

- 注意点： 缩进要用4个空格！

- Python区分大小写

# 数据类型

## 变量

python 的变量数据类型是可变的，不固定的，成为动态语言

	a = 123
	print(a)
	a = "abc"
	print(a)
	
	#会打印出来
	123
	abc
	
a的数据类型改变了。这是允许的。但是在Java oc中是不可以的。

## 常量

在Python中，通常用全部大写的变量名表示常量

	PI = 3.1415926

其实实际意义上 PI 仍然是一个变量，Python不会保证PI的值不会被改变。全部大写只是一个习惯写法，改变PI的值也没问题

## 地板除

在Python中有两种除法， 浮点型的除法 /    整数除法 //

	print(10 / 3)
	#得出结果 3.3333333333333335

即使除数和被除数都是整型，除完之后也是浮点型。即便是能整除
	
	print(9 / 3)
	#得出结果 3.0
	
要想得到跟oc中 / 除法一样的整型相除得整型的除法结果，就要用地板除 //

	print(10//3)
	print(9//3)
	#两个打印结果都是  3

# 字符串和编码

字符串类型 str

% 运算符用作格式化字符串，  %s 字符串， %d 整数， %f 浮点数。 如果不确定数据类型，%s 可以用作任何数据类型，它会把别的数据类型转为字符串

# list 和 tuple

## list 数组

list是一种有序集合，就是数组。list相比于oc中的NSMutableArray有很多独有的特性。比NSMutableArray更加易操作，有更多的功能。swift中新的数据类型元组貌似就借鉴了Python中的list部分功能
	
	classname = ['abc', 'xyz','bob']

获取list元素个数 
	
	len(classname)

角标索引访问某个元素

	classname[0]

Python有一种比较特殊的元素访问方式，以负数做索引。 比如 classname[-1], 这是访问最后一个元素，也就是倒数第一个元素。

以类推，访问倒数第二个元素 classname[-2], 访问倒数第三个元素classname[-3]

当然数组角标索引不能越界！

list数组是可变的

***

追加元素

	classname.append('admin')
	
	print(classname) #结果['abc', 'xyz', 'bob', 'admin']


插入元素，插入到索引为1的位置

	classname.insert(1, 'jack')

删除指定索引的list元素

	classname.pop(0)
	
	#删除最后一个元素
	classname.pop()

替换某个索引的元素值，直接给该索引元素赋值

	classname[0] = 'mak'
	
list内的元素类型也可以不同，不像oc似的必须为对象类型。而且，也可以编辑某一个元素，改变其数据类型
	
	classname = ['abc', 123, True]
	
	classname[1] = 'efg'
	
list嵌套， list里的元素也可以是一个list。 可以用类似oc中字典一层一层往下找的办法，获取list里面list里的元素

	classname = ['abc', 123, True, [1, 'bc', 'ff']]
	
	# 获取打印 ff。 classname这个list里的第三个元素里的第二个元素
	print(classname[3][2])

空list也是可以的，长度为0

	classname = []
	
	len(classname)  # 0
	
## tuple元组

tuple 不可变，创建时用(), 创建时内容就要确定下来，而且后面不能更改

	tuples = (1,2,3)
	
也可以创建空的
	
	tuples = ()
	
注意点是创建只有一个元素的tuple

	#如果写成这样的话，会容易误解，因为()也是数学公式中的小括号，这种情况下，Python会当做小括号处理
	
	tuples = (1) 就变成了 tuples = 1
	
如果想创建一个只有一个元素的tuple，需要这样写:

	tuples = (1,)
	
	#打印出来的也是(1,)  这是为了避免出现歧义


tuple 元素的可变现象， 不是说tuple只要定义了就不能变了吗？为什么还有可变现象？看下面代码

	tuples = (1,2, ['a', 'b'])
	tuples[2][0] = 'x'
	tuples[2][1] = 'y'
	tuples[2].append('f')
	print(tuples)
	
	#打印出来的tuples是  (1, 2, ['x', 'y', 'f'])

表面上看tuple内的元素变了，其实并不是tuple变了，而是tuple里面的list变了。tuple内部每个元素的指向并没有发生变化，指向1的还是指向1，指向2的还是指向2，指向list的还是指向list


# 条件判断

if else 的条件判断，执行流程肯定是国际惯例，区别只是写法不同

	a = 1
	if a > 0:
	    print(a)
	    print('a是正整数')
	else:
	    print(-a)

缩进去的是代码块，可以有多行，都是一块代码。
: 千万不能少些，缩进千万不能省略，而且建议用四个空格缩进。

***

还有一种写法不同，就是else if 需要简写为elif

	age = 200
	if age < 10:
	    print('小于10')
	elif age > 100:
	    print('大于100')
	else:
	    print('其他')
	    

if判断条件简写
	
	a = 1
	if a:
	    print('a非零')
	    
非零、非空即真的理念同样适用于Python。只是不适用于swift，其他的貌似都适应吧

***

一个错误的判断条件

	a = input('请输入一个数字')
	if a > 0:
		print('正数')
	else:
		print('负数')
		
报错了。

	Traceback (most recent call last):
	  File "./hello.py", line 5, in <module>
	    if a > 0:
	TypeError: unorderable types: str() > int()

一个str类型的和一个int类型的做 > 比较， 所以报错了。

用int()函数将str类型转为整数类型，再比较就OK了

	a = int(input('请输入一个数字'))
	if a > 0:
		print('正数')
	else:
		print('负数')

但这时如果输入一个字符a，还是会报错。 后面会讲到错误和调试，检查并捕获程序运行时的错误。


# 循环

## for-in 循环

for-in 遍历list里的每一个元素

	name = ['xiao', 'ming', 'haha']	
	for a in name:
		print(a)

range()函数。 可以生成一个整数数列，比如 range[5] 是从0到5(不包括5)的整数

把range()函数生成的数列转为list  list(range(5))

	print(list(range(5)))
	#打印出来是 [1,2,3,4]

## while循环

	n = 0
	while n < 10:
		print(n)
		n = n+1
注意点： n++不适用，会语法报错。 ++n会死循环输出0

# 字典dict 和 set

## dict

dict 字典是以key-value键值方式存储数据的一种集合。oc中的字典NSDictionary。Java中一般称为key-value数组。

初始化一个dict

	d = {'xiaoming': 10, 'xiaohua': 20, 'xiaoli': 'dd'}

一个key只能对应一个value，相同的key后面的值会把前面的值冲掉

	d = {'xiaoming': 10, 'xiaohua': 20, 'xiaoming': 80}
	print(d)
	#这句打印出来的结果为： {'xiaoming': 'dd', 'xiaohua': 20}
	
可以根据key修改对应value的值，也可以增加新的key-value键值对
	
	d = {'xiaoming': 10, 'xiaohua': 20, 'xiaoli': 'dd'}
	d['xiaoli'] = 30  #修改xiaoli 为 30
	d['xiaowang'] = 80 #新增一个数据xiaowang: 80

根据不存在的key读取value会报错，判断key是否存在的方法：
	
	if 'xiaoming' in d:
	    print('yes')
	判断'xiaoming'是不是dict d 中的key  'xiaoming' in d 如果是True，如果不是False
	
第二种判断key是否存在的方法

	print(d.get('xiaoming'))
	#如果有xiaoming这个key 则返回对应的value，如果没有则返回None。
	
	#或者如果没有返回指定的值
	print(d.get('xiaoming', -1))
	#如果没有，返回指定的-1
	
删除某个key-value键值对

	d.pop('xiaoming')
	#如果key不存在，也会报错
	
**注意: dict内部数据的顺序跟key放入顺序没有关系。是无序的**
**注意: dict的key必须为不可变对象，只要不可变就行(数字也行)。list是可变的，不能作为key**

## set

set是一组key的集合，没有value。之所以说它是一组key，是因为set里面存储的东西不重复、不可变、无序

创建set， 需要提供一个list作为输入集合。但是set里面的元素顺序跟list元素顺序没有关系。
	
	s = set([1,2,3])

单个添加key的方法, 不能一次添加多个。允许添加重复的key，但是无效果。
	
	s.add(4)
	
删除元素，删除无效的key会报错

	s.remove(4)

set的不重复特点可以做两个set的交集并集操作

	s = set([1,2,3,3])
	w = set([3,2,4,7])
	
	print(s & w, s | w) #打印的是交集 和 并集
	
set内的元素也是不可变的，不能存放list

## 不可变对象的永远不可变性

字符串是不可变的对象

	>>> a = 'abc'				#创建字符串
	>>> a.replace('a', 'A')    #将字符串中的a字符替换为A
	'Abc'						#得到结果
	>>> a
	'abc'						#打印a还是'abc'
	>>> 

为什么a的值没有改变？

其实内存中是这样运行的，a是一个变量，'abc'是字符串对象，a这个变量指向了'abc'字符串对象。

执行 a.replace('a', 'A') 操作时，replace方法作用于字符串对象'abc', 'abc'是不可变的字符串对象，replace改变不了其中某个字符，其实是创建了一个新的字符串'Abc'返回了。

下面代码说明了这个原理：

	>>> a = 'abc'					#创建字符串
	>>> b = a.replace('a', 'A')		#将字符串中的a字符替换为A  用b指向这个新串
	>>> b
	'Abc'							#打印b是'Abc'
	>>> a
	'abc'							#打印a是'abc'
	>>> 


**对于不变的对象，调用任一方法操作对象，对象自身内容都不会改变，这些方法会创建新的对象并返回。这样保证了不可变对象本身的永远不可变性**


