# 模块

把代码放到不同的`.py`文件，一个`.py`文件是一个模块(Module)。有Python的内置模块和第三方模块。

不同模块间不必考虑函数名变量名冲突，因为不同模块间完全允许函数名变量名相同。注意不要与内置函数名冲突。

避免模块名冲突，引入目录来组织模块，称为包(Package)。

![img](https://github.com/mxdios/notebook/blob/master/notebooks/images/QQ20160816-0.png?raw=true)

引入包，顶层包名不冲突，那么里面所有的模块都不会冲突。例如`hello.py`模块，引入包`PythonTest`中，模块名就变成了`PythonTest.hello`。

注意：每个包目录下面都有一个`__init__.py`文件，这个文件必须存在，不然这就是个普通目录，而不是一个包。`__init__.py`是空文件也可以写代码，他的模块名是`PythonTest`。

注意：创建模块命名时，不能与自带模块名称冲突。例如系统自带`sys`模块，自己的模块不能命名为`sys.py`，否则将无法导入系统自带`sys`模块。

如果有多层级的包结构，就一路`.`下去。

## 使用模块

Python有很多内置模块，安装完毕即可使用

```Python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-

'a test module'

__author__ = 'MXD'

import sys

def test():
	args = sys.argv
	if len(args) == 1:
		print('hello world!')
	elif len(args) == 2:
		print('hello %s!' % args[1])
	else:
		print('end')

if __name__ == '__main__':
	test()
```

### 标准模板

`#!/usr/bin/env python3` 标准注释，可以让这个模块文件直接在Unix/Linux/Mac上运行

`# -*- coding: utf-8 -*-` 标准注释，表示该文件使用标准UTF-8编码

`'a test module'` 表示该模块的文档注释，任何模块代码的第一个字符串都会被视为模块的文档注释

`__author__ = 'MXD'` 作者

以上是Python模块的标准模板，不是强制必写，写总是没错的

### 代码部分

下面是代码部分

`import sys` 导入`sys`模块。导入模块之后就可以用了，相当于有了变量`sys`指向该模块，利用这个变量可以访问`sys`模块中的所有功能。

`sys`模块有一个`argv`变量，是用list存储了在命令行中输入的所有参数。例如在命令行中输入如如下第一行代码，这是运行`hello.py`模块，这时`argv`中存的就是`['hello.py']`。`argv`中至少有一个元素，因为第一个元素永远是该模块名

```Pythone
mxdMacBook-Pro-3:PythonTest inspiry$ python3 hello.py
hello world!
mxdMacBook-Pro-3:PythonTest inspiry$ python3 hello.py mxd
hello mxd!
mxdMacBook-Pro-3:PythonTest inspiry$ python3 hello.py mxd,xx
hello mxd,xx!
mxdMacBook-Pro-3:PythonTest inspiry$ python3 hello.py ['a', 'adf']
end
mxdMacBook-Pro-3:PythonTest inspiry$ python3 hello.py mxd adf
end
```

### 最后两行代码的详细解释

注意最后的代码

```Python
if __name__ == '__main__':
	test()
```

在命令行中运行`hello`模块文件时，Python解释器会把特殊变量`__name__`置为`__main__`，如果在其他地方导入该	`hello`模块时，`if`判断将失败。这种`if`测试可以让一个模块通过命令行运行时执行一些额外代码，最常见的就是运行测试。

下面详细记录我对这两行代码的作用理解。

注释掉这两行代码，再在终端上执行`python3 hello.py`

```Python
mxdMacBook-Pro-3:PythonTest inspiry$ python3 hello.py
mxdMacBook-Pro-3:PythonTest inspiry$ 
```

不会有任何结果返回。

因为在终端执行该模块，该模块中只定义了一个`test`函数，并没有调用操作。如果这时放开`test()`这一行代码，调用该函数。

```Python
mxdMacBook-Pro-3:PythonTest inspiry$ python3 hello.py
hello world!
```

调用成功！

但是，在启动Python交互环境的时候，导入该模块时，也会调用该函数：

```Python
mxdMacBook-Pro-3:PythonTest inspiry$ python3
Python 3.5.2 (v3.5.2:4def2a2901a5, Jun 26 2016, 10:47:25) 
[GCC 4.2.1 (Apple Inc. build 5666) (dot 3)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> import hello
hello world!
```

这就非我所愿了，我们想要的是我在调用模块的时候返回执行结果，我导入模块的时候不能调用函数。这就用到了注释掉的判断条件`if __name__ == '__main__':`

放开这行注释的判断条件，在Python交互环境下的执行结果：

```Python
mxdMacBook-Pro-3:PythonTest inspiry$ python3
Python 3.5.2 (v3.5.2:4def2a2901a5, Jun 26 2016, 10:47:25) 
[GCC 4.2.1 (Apple Inc. build 5666) (dot 3)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> import hello
>>> hello.test()
hello world!
>>> 
```

还是上文的解释：**在命令行中运行模块文件时，Python解释器会把特殊变量`__name__`置为`__main__`，而在导入模块时则不会有这种改变。**

## 作用域

正常的函数和变量名是公开的(public)，可以被直接引用，比如`abc`, `m322`等；

双下划线开头双下划线结尾的变量`__xxx__`是特殊变量，有特殊用途，自定义变量不要用这种变量名;

单下划线或双下划线开头无下划线结尾的函数或变量名是非公开的(private),不应该被直接引用，比如`_abc`, `__xyz`;

只是"不应该"被直接引用，并不是"不能"被直接引用。Python并没有一种方法能完全限制访问非公开变量或函数。只是编程习惯上不应该这么引用。

使用案例：

```Python
def _abc(name):
	print('hello %s' % name)

def _xyz(name):
	print('biu %s' % name)

def test(name):
	if len(name) > 3:
		return _abc(name)
	else:
		return _xyz(name)
```

这是一个自定义模块，`test`函数是公开的函数，`_abc`和`_xyz`函数是内部函数实现，不需要调用。在调用的时候只需要调用公开的`test`函数即可，不需要关心内部实现细节。调用结果：

```Python
>>> hello.test('aa')
biu aa
>>> hello.test('aaaa')
hello aaaa
>>> 
```

这是一种非常有用的代码封装和抽象方法，外部不需要引用的函数全部定义为未公开的(private)，只有外部需要引用的函数才定义为`public`。

当然，坚持调用`_abc`函数也是可以的

```Python
>>> hello._abc('aaaa')
hello aaaa
>>> 
```

只是不建议这么做。

## 安装第三方模块

安装第三方模块，需要通过包管理工具`pip`完成。Mac或Linux不需要手动安装`pip`，Mac或Linux上可能并存Python3.x和Python2.x版本，对应的pip命令是`pip3`

安装第三方库`Python Imaging Library`，这是Python下的图像处理工具库，目前`PIL`只支持到Python2.7，并且很久没更新。基于PIL的Pillow项目非常活跃，支持Python3.

安装Pillow

```Python
pip3 install Pillow
```

安装完成后，使用Pillow生成图片缩略图

```Python
>>> from PIL import Image
>>> im = Image.open('img.png')
>>> print(im.format, im.size, im.mode)
PNG (1920, 2172) RGBA
>>> im.thumbnail((100,200))
>>> im.save('newimg.jpg', 'JPEG')
>>> 
```

其他常用的第三方库还有MySQL的驱动：`mysql-connector-python`，用于科学计算的NumPy库：`numpy`，用于生成文本的模板工具`Jinja2`，等等。





