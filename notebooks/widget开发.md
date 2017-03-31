widget是iOS8时推出的窗口小部件功能，窗口小部件在Android上早已大行其道。记得当年用过的第一部Android是深圳出产的国产机，当时滑过三四个屏幕的应用，还能继续再滑三四个屏幕的窗口小部件。用的最多的窗口小部件就是日历了，屏幕上一目了然。

Apple直到iOS8才加入窗口小部件，而且可自定义程度远远没有Android开放。

本文记录了开发widget的步骤，以及遇到的一些问题。

开发环境：Xcode8.2.1，swift3.0

## 创建widget

widget可以理解为一个独立的项目，虽然形式上看来像是附属于app的一部分功能，其实并不是，widget想获取app的数据，还需要做数据共享。

`File` -> `New` -> `Target`

![创建Today Extension](https://github.com/mxdios/notebook/blob/master/notebooks/images/2016-10-094.36.08.png?raw=true)

选择iOS里的`Today Extension`

![创建Today Extension](https://github.com/mxdios/notebook/blob/master/notebooks/images/2016-10-094.40.27.png?raw=true)

习惯使用纯代码布局，喜欢用storyboard的不需要下面的info.plist修改。在新创建的widget项目文件夹中删除`MainInterface.storyboard`，修改info.plist里的`NSExtension`字段：

1. 删除`NSExtensionMainStoryboard`字段
2. 添加`NSExtensionPrincipalClass`字段，Value 为`TodayViewController`（`TodayViewController`是自定义控制器，）

修改info.plist的结果如下

![img](https://github.com/mxdios/notebook/blob/master/notebooks/images/QQ20161009-0.png?raw=true)

### 问题1.widget崩溃

经过上述修改，用纯代码布局widget，用OC开发是没有问题的，swift3.0中widget会崩溃，并打印下面的错误。

```
*** Terminating app due to uncaught exception 'NSInvalidArgumentException', reason: '*** setObjectForKey: object cannot be nil (key: 56A34ADC-7A43-43B0-A924-171F803DD305)'
libc++abi.dylib: terminating with uncaught exception of type NSException
```

StackOverflow有人遇到同样的问题：[Today Extension Crashes before launching on iOS 8.1.2](http://stackoverflow.com/questions/28153273/today-extension-crashes-before-launching-on-ios-8-1-2)，但解答好像并没有效果。

在一篇博客中我找到了解决办法，[博客地址](http://blog.xebia.com/ios-today-widget-written-in-swift/)

```
Since (at the time of this writing) Xcode cannot find Swift classes as extension principal classes, we also would have to add the following line to our TodayViewController:

@objc (TodayViewController)

Update: Make sure to set the "Embedded Content Contains Swift Code" build setting of the main app target to YES. Otherwise your widget written in Swift will crash.
```

文中说目前为止Xcode找不到swift类作为拓展主题类（其实到我写这篇文章的时候，还是找不到），这可能是一个bug。解决办法是需要在widget控制器`TodayViewController`中添加：

```swift
@objc (TodayViewController)
```

博文更新中说，可以更改`Embedded Content Contains Swift Code`这个设置为yes，但是在Xcode8.2.1中，这个设置已经没有了，取而代之的是`Always Embed Swift Standard Libraries`，亲测主`targets`和widget的`targets`中修改这个设置的Bool值，都还是会崩溃。

## widget折叠

iOS10之后才有的widget折叠。

```swift
if #available(iOSApplicationExtension 10.0, *) {
    self.extensionContext?.widgetLargestAvailableDisplayMode = .expanded
} else {
    // Fallback on earlier versions
}
```

实现下面方法。

```swift
@available(iOSApplicationExtension 10.0, *)
func widgetActiveDisplayModeDidChange(_ activeDisplayMode: NCWidgetDisplayMode, withMaximumSize maxSize: CGSize) {
	if activeDisplayMode == .expanded {
        self.preferredContentSize = CGSize(width: 0, height: 200)
	} else {
			self.preferredContentSize = maxSize
	}
}
```

## 代码共享

虽然widget附属于主应用，但其实是独立的。在widget中无法调用主应用中的代码，这样一来就蛋疼了。有些公共方法或者控件，在主应用中写完了，在widget却无法使用。当然把主应用中的代码拷贝一份到widget中也是可以的，这种做法太low。

可以使用framework做代码共享。创建一个framework

`File` -> `New` -> `Target`

![创建framework](http://oalg33nuc.bkt.clouddn.com/QQ20170330-150955.png)

在`framework`的`Build Phases` -> `Compile Sources`里面添加要共享的代码文件。

![添加共享的代码文件](http://oalg33nuc.bkt.clouddn.com/QQ20170330-152310.png)

在`TARGETS`里面，分别在主项目和widget下面的`Linked Frameworks and Libraries`里面添加新建的`framework`

并在widget中用到共享代码的地方引入`framework`

```swift
import ShareToday
```

### 问题2.引入framework报错和报警告

引入的时候会如下错误：
```
TodayViewController.swift:11:8: Module file's minimum deployment target is ios10.0 v10.0:
```

是因为framework的`Deployment Target`的版本号和widget的版本号不相符，改为一样的即可。

报如下警告：

```
ld: warning: linking against a dylib which is not safe for use in application extensions: 
```

是因为application extensions限制了一些API的使用，而在新建的framework里面，可能包含了这些API，所以才会出现这个警告。

解决办法：勾选framework里面的` Allow app extension API only`

![注意](http://oalg33nuc.bkt.clouddn.com/QQ20170330-152418.png)

### 问题3.方法调用不到

加入到framework的一些方法，在引入头文件后的widget调用不到。

解决办法：需要把方法设置为公用的，用`public`修饰方法，例如

```swift
public func getString(a: Int) -> String {
	  return "\(a)"
}
```




### 点击widget开启app

在app的`TARGEST`里的`info`下`URL Types`添加`URL Schemes`

![img](https://github.com/mxdios/notebook/blob/master/notebooks/images/QQ20161009-1.png?raw=true)

widget上显示的自定义view：`_calendarView`，给`_calendarView`添加触摸事件。

```Objective-C
UITapGestureRecognizer *tap = [[UITapGestureRecognizer alloc] initWithTarget:self action:@selector(openApp)];
[_calendarView addGestureRecognizer:tap];
```

实现触摸事件，开启app

```Objective-C
- (void)openApp {
    [self.extensionContext openURL:[NSURL URLWithString:@"paibanapp://"] completionHandler:^(BOOL success) {
        NSLog(@"successs = %d", success);
    }];
}
```

### 数据共享

创建App Groups的Identifiers。在创建证书的时候勾选App Groups，设置创建的App Groups

![img](https://github.com/mxdios/notebook/blob/master/notebooks/images/QQ20161009-3.png?raw=true)

在Xcode的`TARGEST` -> `Capabilities`里打开App Groups。

![img](https://github.com/mxdios/notebook/blob/master/notebooks/images/QQ20161009-2.png?raw=true)

#### 用NSUserDefaults共享数据，

存储数据

```Objective-C
NSUserDefaults *shared = [[NSUserDefaults alloc] initWithSuiteName:@"group.xxxx"];//App Groups ID
[shared setObject:[NSDictionary dictionaryWithObjectsAndKeys:dutyArray, @"dutyArrayKey", selectDayStr, @"selectDayStrKey", tags, @"selectTagArrayKey", nil] forKey:@"todayViewShared"];
[shared synchronize];
```

读取数据

```Objective-C
NSUserDefaults *shared = [[NSUserDefaults alloc] initWithSuiteName:@"group.xxxx"];//App Groups ID
NSDictionary *dict = [NSDictionary dictionaryWithDictionary:[shared objectForKey:@"todayViewShared"]];
```

#### 用NSFileManager共享数据

存储数据

```Objective-C
NSError *err = nil;
NSURL *containerURL = [[NSFileManager defaultManager] containerURLForSecurityApplicationGroupIdentifier:@"group.xxxx"];//App Groups ID
containerURL = [containerURL URLByAppendingPathComponent:@"Library/Caches/widget"];
NSDictionary *value = [NSDictionary dictionaryWithObjectsAndKeys:dutyArray, @"dutyArrayKey", selectDayStr, @"selectDayStrKey", tags, @"selectTagArrayKey", nil];
BOOL result = [value writeToURL:containerURL atomically:YES encoding:NSUTF8StringEncoding error:&err];
if (!result) {
	NSLog(@"%@",err);
} else {
	NSLog(@"save value:%@ success.",value);
}
```

读取数据

```Objective-C
NSError *err = nil;
NSURL *containerURL = [[NSFileManager defaultManager] containerURLForSecurityApplicationGroupIdentifier:@"group.xxxx"];//App Groups ID
containerURL = [containerURL URLByAppendingPathComponent:@"Library/Caches/widget"];
NSString *value = [NSString stringWithContentsOfURL:containerURL encoding: NSUTF8StringEncoding error:&err];
```

