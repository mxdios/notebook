# runtime的消息传递

一直以来对runtime都不了解，今天大致看了一下，做一下笔记。

## 方法调用

oc中习惯称函数为方法，`-`开头的为实例方法，`+`开头的为类方法。方法调用在oc开发中最为常用，将一个功能实现封装为一个方法，然后调用拿到想要的结果，这也是面向对象编程思想。

如果调用的方法不存在怎么办？—— 程序崩溃。

比如在一个控制器的`- (void)viewDidLoad`方法中调用`test`方法。

```Objective-c
- (void)viewDidLoad {
    [super viewDidLoad];
    [self performSelector:@selector(test)];
}
```
`test`方法并没有写，运行起来程序肯定崩溃，控制台打印的错误信息是比较常见的：

```
-[ViewController test]: unrecognized selector sent to instance 0x7f93ab805990
```

其实在找不到方法的时候并不会立即崩溃抛出错误，而是会执行几个方法：

```Objective-c
+ (BOOL)resolveInstanceMethod:(SEL)sel
- (id)forwardingTargetForSelector:(SEL)aSelector
- (NSMethodSignature *)methodSignatureForSelector:(SEL)aSelector
- (void)forwardInvocation:(NSInvocation *)anInvocation
```

程序找不到方法会调用下面方法：

```Objective-c
- (void)doesNotRecognizeSelector:(SEL)aSelector {
    NSLog(@"%@", NSStringFromSelector(aSelector)); //输出找不到的方法名
}
```

## + (BOOL)resolveInstanceMethod:(SEL)sel

这个方法在调用test方法且test方法不存在时会调用，程序运行在执行`- (void)viewDidLoad`之前也会调用一次(加载Storyboard)，如果存在test方法则不会调用。

在ViewController里面实现如下方法：

```Objective-c
- (void)viewDidLoad {
    [super viewDidLoad];
    NSLog(@"加载viewDidLoad");
    [self performSelector:@selector(test)];
}
+ (BOOL)resolveInstanceMethod:(SEL)sel {
    NSLog(@"调用1 = %@", NSStringFromSelector(sel));
    return [super resolveInstanceMethod:sel];
}
```

运行结果：

```
2016-09-22 15:16:01.671 runtimeTest[9672:285711] 调用1 = setStoryboard:
2016-09-22 15:16:01.675 runtimeTest[9672:285711] 加载viewDidLoad
2016-09-22 15:16:01.675 runtimeTest[9672:285711] 调用1 = test
2016-09-22 15:21:12.351 runtimeTest[9727:292932] -[ViewController test]: unrecognized selector sent to instance 0x7ff2d0e05e20
```
当然程序最终结果还是崩溃了，但是可以看到程序进入了`+ (BOOL)resolveInstanceMethod:(SEL)sel`方法，并且打印出来的`sel`就是我们要调用的`test`。



