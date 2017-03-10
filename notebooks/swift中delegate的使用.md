delegate是iOS开发中一种非常常用的设计模式，经常用作界面的反向传值。block可以做到同样的事情，曾有人言delegate是个臃肿的胖子，远不如block轻便，早该淘汰了。在ARC中，block中使用swift要用weak修饰，注意循环引用。delegate也需要使用weak指针，避免内存泄露。

```Objective-c
@property (nonatomic, weak)id<XDViewControllerDelegate> delegate;
```
ARC环境下在oc中定义delegate使用weak修饰，在delegate对象释放掉后，delegate没有强指针引用也会被释放掉，避免了访问不存在delegate对象的delegate方法而崩溃。可以在`dealloc`方法中做防御性操作：置空delegate。

```Objective-c
- (void)dealloc {
    self.delegate = nil;
}
```

在最近的swift项目中用到delegate，由于没有使用weak修饰，控件循环引用无法释放，导致内存泄露。

场景是这样的: 在控制器中添加了A控件，A控件中有个子控件B，B有个代理方法，A作为他的代理，并实现了代理方法。在完成功能后，使用`removeFromSuperview()`移除A控件时，发现A和B的反初始化方法`deinit`都没调用，说明这两个控件没被释放掉，内存泄露了。

*这里有个小坑，`deinit { }`方法里面如果没有执行代码是不会调用的，使用`deinit`检测该控件是否被移除，记得在`deinit { }`里面写一行执行代码，比如`print("deinit")`*

因为我在定义delegate的时候，是这样定义的：

```Objective-c
protocol workSelectViewDelegate {
    func addWorkBtnViewClick(workTitle: String, workColor: String)
}
class WorkSelectView: UIView{
    var delegate:workSelectViewDelegate?
}
```
由于delegate没有weak修饰，在移除A控件的时候，A中有B，B的代理又是A，A和B相互引用，导致谁也没能释放掉。

将delegate使用weak修饰即可，直接在`var delegate`前面加`weak`，编译会报错。这是因为在swift中遵守protocol的类型有很多，其中有些类型不支持weak修饰，比如struct。这里需要限制protocol的类型遵守。比如下面指定这个protocol只能由class遵守：

```Objective-c
protocol workSelectViewDelegate: class {
    func addWorkBtnViewClick(workTitle: String, workColor: String)
}
class WorkSelectView: UIView{
    weak var delegate:workSelectViewDelegate?
}
```

还有一种方式是定义oc类型的protocol，因为oc类型的protocol只有class实现。使用关键词`@objc`修饰protocol：

```Objective-c
@objc protocol workSelectViewDelegate {
    func addWorkBtnViewClick(workTitle: String, workColor: String)
}
class WorkSelectView: UIView{
    weak var delegate:workSelectViewDelegate?
}
```

相比第二种方法，第一种方法明显才是swift该有的样子，第二种太依赖oc了。

说起依赖oc，想起来在delegate定义中的`optional`，在swift中定义delegate方法，如果没有可选修饰，遵守了代理而不实现代理方法，不是警告，而是直接报错。

要想使用可选实现的修饰`optional`，必须使用`@objc`修饰

```Objective-c
@objc protocol workSelectViewDelegate{
    func addWorkBtnViewClick(workTitle: String, workColor: String)
    @objc optional func addTimeTextString()
}
```

这样还有一个问题，如果代理有结构体作为返回值的话，会不支持而报错。

需要另一种方法定义可选代理方法，协议拓展：

```Objective-c
protocol workSelectViewDelegate: class{
    func addWorkBtnViewClick(workTitle: String, workColor: String)
    func addTimeTextString()
}
extension workSelectViewDelegate {
    func addTimeTextString() {
        
    }
}
```


