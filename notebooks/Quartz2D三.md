## Pattern

Pattern可翻译为样式、模型。类似于iOS中自定义的view，可以重复多次把自定义的view添加到控制器的view上。样式(Pattern)是绘制操作的一个序列，可以重复的绘制到一个图形上下文上。

### 样式单元格

样式单元格是样式的基本组件，在创建的时候就要定义单元格的边界，并在这个范围内进行绘制。在图形上下文上绘制单元格时，可以随意指定两个单元格之间上下左右间距，也可以指定负数，单元格重合而已。像极了添加view控件。

### 着色样式和模板样式

着色样式是有固定颜色的，在创建样式单元格的时候就指定其颜色了，跟绘制流程没有关系。

模板样式即为非着色样式，在创建样式单元格的时候只指定了形状，并没有指定颜色，在绘制过程中指定颜色。

在Quartz 2D中可以使用这两种样式实现样式绘图，将创建好的样式绘制到page上的过程称为平铺，有三种平铺选项：1、没有失真，2、最小的失真的恒定间距，3、恒定间距。

### 绘制着色样式

绘制着色样式一般需要下面几步：1、设置样式单元格的回调函数，2、设置着色模式单元格，3、绘制操作。下面是绘制着色样式的示例代码和运行结果：

```Objective-C
- (void)drawRect:(CGRect)rect {
    [super drawRect:rect];
    CGContextRef context = UIGraphicsGetCurrentContext();
    clorePatternPaingting(context, CGRectMake(0, 0, 300, 500));
}
void clorePatternPaingting(CGContextRef context, CGRect rect) {
    CGColorSpaceRef patternSpace = CGColorSpaceCreatePattern(NULL);
    CGContextSetFillColorSpace(context, patternSpace);
    CGColorSpaceRelease(patternSpace);
    static const CGPatternCallbacks call = {0, &drawPattern, NULL};
    CGPatternRef pattern = CGPatternCreate(NULL, CGRectMake(0, 0, 30, 30), CGAffineTransformIdentity, 30, 30, kCGPatternTilingConstantSpacing, true, &call);
    CGFloat alpha = 1.0;
    CGContextSetFillPattern(context, pattern, &alpha);
    CGPatternRelease(pattern);
    CGContextFillRect(context, rect);
}
void drawPattern(void *info, CGContextRef context) {
    CGContextSetRGBFillColor(context, 1, 0, 0, 1);
    CGContextFillRect(context, CGRectMake(0, 0, 10, 10));
    CGContextSetRGBFillColor(context, 0, 1, 0, 1);
    CGContextFillRect(context, CGRectMake(10, 0, 10, 10));
    CGContextSetRGBFillColor(context, 0, 0, 1, 1);
    CGContextFillRect(context, CGRectMake(0, 10, 10, 10));
    CGContextSetRGBFillColor(context, 0.5, 0, 0.5, 1);
    CGContextFillRect(context, CGRectMake(10, 10, 10, 10));
}
```

运行结果：

![绘制着色模式的运行结果](http://oalg33nuc.bkt.clouddn.com/QQ20161220-0.png)

**代码解释：**

上述代码块中第三个函数`drawPattern`的作用就是第1步：设置样式单元格的回调函数。参数`info`:是指向模式相关数据的指针，可以传`NULL`。`context`:是图形上下文。在该函数中指定了每个颜色块的大小和填充色。因为是着色样式，所以指定填充色。

第二个函数，也是在`drawRect`方法中调用的函数`clorePatternPaingting`，执行了第2步:设置着色模式单元格和第3步:绘制操作。下面对每块代码进行解释：

**清空基本颜色空间：**

```Objective-C
CGColorSpaceRef patternSpace = CGColorSpaceCreatePattern(NULL);
CGContextSetFillColorSpace(context, patternSpace);
CGColorSpaceRelease(patternSpace);
```

因为我们要使用着色样式里面的颜色，所以清空基本颜色空间，以确保着色样式上的颜色能显示。可以试一下把这段代码注释的结果，绘制着色样式的空间为黑色，因为基本颜色空间没清空也没着色，所以呈黑色。

**设置着色模式单元格：**

```Objective-C
static const CGPatternCallbacks call = {0, &drawPattern, NULL};
CGPatternRef pattern = CGPatternCreate(NULL, CGRectMake(0, 0, 30, 30), CGAffineTransformIdentity, 30, 30, kCGPatternTilingConstantSpacing, true, &call);
```

模式单元格的信息保存在`CGPatternRef`类型的`CGPattern`对象中，使用`CGPatternCreate`创建该对象。需要传入的参数依次解释为：

1.	info：是一个指针，指向我们要传递给绘制回调函数的数据。这里传的是`NULL`
2.	bound：指定模式单元格的大小
3.	matrix：指定模式矩阵，它将模式坐标系统映射到图形上下文的默认坐标系统。如果希望两个坐标系统是一样的，则可以使用单位矩阵。
4.	xStep, yStep：指定单元格之间的水平和竖直间距。
5.	tiling：平铺模式，可以是kCGPatternTilingNoDistortion、kCGPatternTilingConstantSpacingMinimalDistortion、kCGPatternTilingConstantSpacing
6.	isColored：指定模式单元格是着色模式(true)还是模板模式(false)
7.	callbacks：是一个指向CGPatternCallbacks结构体的指针。格式如下：

```Objective-C
struct CGPatternCallbacks {
    unsigned int version;
    CGPatternDrawPatternCallback __nullable drawPattern;
    CGPatternReleaseInfoCallback __nullable releaseInfo;
};
```

`version`一般设置为`0`，`drawPattern`是指向设置样式单元格的回调函数，`releaseInfo`是指向回调函数，释放CGPattern对象时调用，以释放存储在我们传递给绘制回调的info参数中的数据。如果在这个参数中没有传递任何数据传`NULL`。

所以`CGPatternCallbacks`结构体定义为`static const CGPatternCallbacks call = {0, &drawPattern, NULL};`

**绘制着色样式**

```Objective-C
CGFloat alpha = 1.0;
CGContextSetFillPattern(context, pattern, &alpha);
CGPatternRelease(pattern);
CGContextFillRect(context, rect);
```

调用函数`CGContextSetFillPattern`使用着色样式进行填充操作，或者调用函数`CGContextSetStrokePattern`进行描边操作。需要指定着色样式在填充或描边过程的透明度。

样式使用完毕后需要释放，调用函数：`CGPatternRelease`

将定义的着色样式使用函数`CGContextFillRect`填充到指定`rect`的矩形中。

这一系列操作都放到函数`clorePatternPaingting`内，在`drawRect`方法中直接调用该函数即可完成着色样式的定义和绘制。
    
### 绘制模板样式

绘制模板样式的步骤和绘制着色样式的一样：1、设置样式单元格的回调函数，2、设置模板模式单元格，3、绘制操作。下面是绘制模板样式的示例代码和运行结果：

```Objective-C
- (void)drawRect:(CGRect)rect {
    [super drawRect:rect];
    CGContextRef context = UIGraphicsGetCurrentContext();
    stencilPatternPaingting(context, CGRectMake(0, 0, 300, 500));
}
void stencilPatternPaingting(CGContextRef context, CGRect rect) {
    CGColorSpaceRef baseSpace = CGColorSpaceCreateDeviceRGB();
    CGColorSpaceRef patternSpace = CGColorSpaceCreatePattern(baseSpace);
    CGContextSetFillColorSpace(context, patternSpace);
    CGColorSpaceRelease(patternSpace);
    CGColorSpaceRelease(baseSpace);
    static const CGPatternCallbacks cell = {0, &drawStencilPattern, NULL};
    CGPatternRef pattern = CGPatternCreate(NULL, CGRectMake(0, 0, 30, 30), CGAffineTransformIdentity, 40, 40, kCGPatternTilingConstantSpacing, false, &cell);
    static const CGFloat color[] = {1, 0, 0, 1};
    CGContextSetFillPattern(context, pattern, color);
    CGPatternRelease(pattern);
    CGContextFillRect(context, rect);
}
void drawStencilPattern(void *info, CGContextRef context) {
    CGContextAddEllipseInRect(context, CGRectMake(0, 0, 30, 30));
    CGContextFillPath(context);
}
```

运行结果：

![绘制模板样式的运行结果](http://oalg33nuc.bkt.clouddn.com/QQ20161220-1.png)

代码解释参考上文中着色样式的代码解释。区别1是用下述代码设置样式单元格的颜色：

```Objective-C
static const CGFloat color[] = {1, 0, 0, 1};
CGContextSetFillPattern(context, pattern, color);
```

区别2是`CGPatternCreate`函数中传的参数是`false`。

## 阴影




