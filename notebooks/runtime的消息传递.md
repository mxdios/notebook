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

避免程序崩溃，可以在这个方法中做相应处理，导入`#import <objc/runtime.h>`，改写如下代码：

```Objective-c
+ (BOOL)resolveInstanceMethod:(SEL)sel {
    if (sel == @selector(test)) {
        class_addMethod([self class], sel, (IMP)testIMP, "v@:");
        return YES;
    }
    return [super resolveInstanceMethod:sel];
}
void testIMP(id self, SEL _cmd) {
    NSLog(@"打印test");
}
```

控制台会输出`2016-09-22 15:44:21.231 runtimeTest[9840:314576] 打印test`，程序不会崩溃了。

这里用到了一个`class_addMethod`方法，

```Objective-c
OBJC_EXPORT BOOL class_addMethod(Class cls, SEL name, IMP imp, const char *types) 
```

- `Class cls`在当前类中添加方法，是不是类似`target`?

- `SEL name`方法中的sel

- `IMP imp`自定义的方法

- `*types`定义自定义方法的参数类型和返回值的字符串，`v@:`中的`v`指的是`void`，`@`指参数，`:` 指方法`SEL _cmd`。比如自定义函数：

	```Objective-c
	int newTest(id self, SEL _cmd, NSString *str) {
  		 return 100;
	}
	```
	添加的函数方法应该是：
	
	```Objective-c
	ass_addMethod([self class], @selector(newTest), (IMP)newMethod, "i@:@");
	```

## - (id)forwardingTargetForSelector:(SEL)aSelector

当我们在A控制器中调用B控制器里的方法时，需要将B控制器的方法暴露在.h头文件中，并在A控制器`#import`B控制器头文件，才能调用。当不做这些工作时，调用方法程序就会崩溃。

可以在`- (id)forwardingTargetForSelector:(SEL)aSelector`方法中实现不暴露方法，不引入头文件，也能调用其他控制器里的方法。

如果`+ (BOOL)resolveInstanceMethod:(SEL)sel`方法中没有找到执行方法，消息会继续传递到`- (id)forwardingTargetForSelector:(SEL)aSelector`方法中。

创建一个`testViewController`控制器，在控制器里实现`test2`方法:

```Objective-c
- (void)test2 {
    NSLog(@"hello world !!!!!!");
}
```
在`ViewController`控制器里实现下面方法：

```Objective-c
- (void)viewDidLoad {
    [super viewDidLoad];
    [self performSelector:@selector(test2)];
}
- (id)forwardingTargetForSelector:(SEL)aSelector {
    Class class = NSClassFromString(@"testViewController");
    UIViewController *vc = class.new;
    if (aSelector == NSSelectorFromString(@"test2")) {
        NSLog(@"hello");
        return vc;
    }
    return nil;
}
```
执行结果：

```
2016-09-22 16:31:35.581 runtimeTest[10033:358140] hello
2016-09-22 16:31:35.581 runtimeTest[10033:358140] hello world !!!!!!
```

程序并没有崩溃，当消息传递到`- (id)forwardingTargetForSelector:(SEL)aSelector`这个方法中时，创建一个`testViewController`对象，并将调用`test2`的消息传递给`testViewController`对象，让`testViewController`去实现`test2`方法，恰好`testViewController`中存在`test2`方法。不知这算不算多继承。

参考资料HenryCheng的简书文章[runtime那些事（消息机制）](http://www.jianshu.com/p/f6300eb3ec3d)


