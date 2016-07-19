# 函数

## 一些函数举例

- 绝对值 abs(-1)

- 最大值 max(1,2,3)

- 类型转换 int('123')  float('12.3') str(100) bool(1)

- 整数转十六进制 hex(10)

给函数起别名

	>>> a = abs
	>>> a(-1)

## 自定义函数

在Python交互环境中自定义函数

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

在.py文件中自定义函数并运行

	def my_abs(x):
	if x >= 0:
		return x
	else:
		return -x

	print(my_abs(10))		#调用函数，并打印出结果
	print(my_abs(-10))
	

将函数封装在一个.py文件，并在Python交互环境中调用

将下面函数代码写入一个.py文件，并将该文件命名为myabs.py

	def my_abs(x):
	if x >= 0:
		return x
	else:
		return -x

python 进入Python交互环境

	>>> from myabs import my_abs  #引入函数  myabs是文件名不带.py， my_abs是文件里的函数名
	>>> my_abs(-5)     			   #调用函数
	5

## 空函数与pass语句

自定义一个空函数

	def nodef():
	    pass

pass 就是什么都不做，用作占位符，能让程序运行起来，不报错。没有它会有语法错误

用Sublime text文本编辑器写好多语句时，都会自动加上pass

## 参数检查

自定义的my_abs()函数相比于系统的abs函数还是不一样。比如传入错误类型的参数'a'

两者报错不同。系统abs函数会检查出参数错误，而自定义的my_abs 是if判断条件错误。所以要增加参数类型检查，判断如果是不符合的参数类型，抛出异常

检查数据类型用isinstance()函数

	def my_abss(x):
	if not isinstance(x, (int, float)):   ##如果不是int和float类型的话，抛出类型异常
		raise TypeError('输入类型错误')
	if x >= 0:
		return x
	else:
		return -x


**写这个的时候遇到了一个问题，一个看代码很难发现的问题**

错误提示如下：
		
	TabError: inconsistent use of tabs and spaces in indentation

在我进行缩进的时候，不同位置用了不同的缩进方式，四个空格和tab共用了，然后就报错了。我尝试了一下，全部使用四个空格缩进和全部使用tab缩进都可以，但是不能混合使用。

