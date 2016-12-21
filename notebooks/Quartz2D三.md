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

阴影有三个属性：x偏移量，y偏移量，模糊度

阴影通过函数`CGContextSetShadow`设置，指定图形上下文、x偏移量、y偏移量、模糊度。设置阴影后，绘制的对象都会有阴影，阴影的色值为0.3透明度的黑色，颜色RGBA值为{0, 0, 0, 0.3}。

也可以是使用函数`CGContextSetShadowWithColor`设置彩色阴影。

在调用`CGContextSetShadow`或`CGContextSetShadowWithColor`之前保存图形状态，可以通过恢复图形状态来关闭阴影。或者设置阴影颜色为`NULL`关闭阴影。

分别设置彩色阴影和普通阴影：

```Objective-C
CGContextRef context = UIGraphicsGetCurrentContext();
CGContextSaveGState(context);
CGContextSetShadowWithColor(context, CGSizeMake(10, 10), 2, [UIColor redColor].CGColor);
CGContextSetRGBFillColor (context, 0, 1, 0, 1);
CGContextAddEllipseInRect(context, CGRectMake(40, 40, 100, 100));
CGContextFillPath(context);
CGContextSetShadow(context, CGSizeMake(10, 10), 2);
CGContextFillRect(context, CGRectMake(200, 40, 100, 100));
CGContextRestoreGState(context);
```

运行结果：

![圆形的红色阴影和矩形的灰色阴影](http://oalg33nuc.bkt.clouddn.com/QQ20161220-2.png)

x偏移量正值在图形右边，负值在图形左边。y偏移量和坐标有关，在iOS中，UIKit坐标系y轴和Quartz 2D坐标系y轴相反，所以如果是由UIKit创建的图形上下文则正值在图形下面，负值在图形上面。macOS的坐标系和Quartz 2D坐标系一样，所以正值在图形上面，负值在图形下面。阴影绘制不受CTM影响。

## 渐变

可以使用函数`CGShadingRef`和`CGGradientRef`创建渐变。渐变分为轴向渐变和径向渐变。渐变的颜色变化有很多，可以是一种颜色到另一种颜色过度，也可以是多种颜色依次过度变化，还有是一种颜色的透明度`alpha`变化，但是这种无法绘制到PDF上，无法打印。

`CGShading`和`CGGradient`的区别：

|CGGradient|CGShading|
|:---|:---|
|可以使用同一对象绘制轴向和径向梯度|需要创建单独的对象的轴向和径向梯度|
|在绘图时设置渐变的几何体|在对象创建时间设置渐变的几何体|
|Quartz计算梯度中每个点的颜色|需要提供一个回调函数，用来计算渐变中每个点的颜色|
|可以定义两个以上的位置和颜色|需要设计回调，使用两个以上的位置和颜色|

### CGGradient

使用CGGradient创建渐变。

1. 首先调用函数`CGGradientCreateWithColorComponents`创建`CGGradient`对象，参数含义分别是：颜色空间、颜色数组（采用RGBA颜色）、位置数组（0.0-1.0之间的数值，0.0为轴线起点，1.0为轴线终点。传递NULL默认0为起始位置，1为终点位置，相当于{0.0, 1.0}）、数组中元素个数。
2. 调用`CGContextDrawLinearGradient`函数绘制轴向渐变或`CGContextDrawRadialGradient`函数绘制径向渐变，参数含义分别是：图形上下文、`CGGradient`对象、起始位置、终止位置、绘图选项。
3. 绘制完毕后，释放颜色空间和`CGGradient`对象。

绘制轴向渐变:

```Objective-C
CGContextRef context = UIGraphicsGetCurrentContext();
CGFloat compoents[] = {1,0,1,1, 0.3,0.5,1,1.0};
CGFloat locations[] = {0.0, 1.0};
CGColorSpaceRef colorSpace = CGColorSpaceCreateWithName(kCGColorSpaceGenericRGB);
CGGradientRef gradient = CGGradientCreateWithColorComponents(colorSpace, compoents, locations, 2);
CGContextDrawLinearGradient(context, gradient, CGPointMake(0, 0), CGPointMake(0, 100), kCGGradientDrawsBeforeStartLocation);
CGColorSpaceRelease(colorSpace);
CGGradientRelease(gradient);
```

![使用CGGradient绘制轴向渐变](http://oalg33nuc.bkt.clouddn.com/QQ20161221-0.png)

绘制径向渐变:

```Objective-C
CGContextRef context = UIGraphicsGetCurrentContext();
CGFloat compoents[] = {1,0,1,1, 0.3,0.5,1,1.0};
CGFloat locations[] = {0.0, 1.0};
CGColorSpaceRef colorSpace = CGColorSpaceCreateWithName(kCGColorSpaceGenericRGB);
CGGradientRef gradient = CGGradientCreateWithColorComponents(colorSpace, compoents, locations, 2);
CGContextDrawRadialGradient(context, gradient, CGPointMake(200, 50), 20, CGPointMake(200, 400), 70, kCGGradientDrawsAfterEndLocation);
CGColorSpaceRelease(colorSpace);
CGGradientRelease(gradient);
```

![使用CGGradient绘制径向渐变](http://oalg33nuc.bkt.clouddn.com/QQ20161221-1.png)

### CGShading

绘制轴向渐变需要以下步骤：

1.	设置 CGFunction 对象来计算颜色值
2.	创建轴向渐变的 CGShading 对象
3.	裁减上下文
4.	使用 CGShading 对象来绘制轴向渐变
5.	释放对象

绘制轴向渐变完整代码：

```Objective-C
- (void)drawRect:(CGRect)rect {
    [super drawRect:rect];
    CGContextRef context = UIGraphicsGetCurrentContext();
    CGColorSpaceRef colorspace = CGColorSpaceCreateDeviceRGB();
    CGFunctionRef functionObj = getFunction(colorspace);
    CGShadingRef shading = CGShadingCreateAxial(colorspace, CGPointMake(0, 0.5), CGPointMake(1, 0.5), functionObj, false, false);
    CGAffineTransform myTransform = CGAffineTransformMakeScale(100, 100);
    CGContextConcatCTM (context, myTransform);
    CGContextSaveGState (context);
    CGContextBeginPath(context);
    CGContextAddArc(context,  .5, .5, .3, 0, M_PI, 0);
    CGContextClosePath(context);
    CGContextClip(context);
    CGContextDrawShading(context, shading);
    CGShadingRelease(shading);
    CGColorSpaceRelease(colorspace);
    CGFunctionRelease(functionObj);
    CGContextRestoreGState (context);
}

static CGFunctionRef getFunction(CGColorSpaceRef colorspace) {
    size_t numComponents = 1 + CGColorSpaceGetNumberOfComponents(colorspace);
    CGFloat input_value_range[] = {0, 1};
    CGFloat output_value_range[] = {0, 1, 0, 0, 0,0,1,1};
    CGFunctionCallbacks callbacks = {0, &calculateShadingValues, NULL};
    return CGFunctionCreate((void *) numComponents, 1, input_value_range, numComponents, output_value_range, &callbacks);
}

static void calculateShadingValues(void *info, const CGFloat *in, CGFloat *out) {
    CGFloat v;
    size_t k, compoents;
    static const CGFloat c[] = {1,0,0.5,1};
    compoents = (size_t)info;
    v = *in;
    for (k = 0; k < compoents - 1; k ++)
        *out ++ = c[k] * v;
    *out++ = 1;
}
```

运行结果：

![使用CGShading绘制轴向渐变](http://oalg33nuc.bkt.clouddn.com/QQ20161221-2.png)

绘制径向渐变，需要以下步骤：

1.	设置 CGFunction 对象来计算颜色值
2.	创建径向渐变的 CGShading 对象
3.	使用 CGShading 对象来绘制径向渐变
4.	释放对象

绘制径向渐变完整代码：

```Objective-C
- (void)drawRect:(CGRect)rect {
    [super drawRect:rect];

    CGContextRef context = UIGraphicsGetCurrentContext();
    
    CGColorSpaceRef colorspace = CGColorSpaceCreateDeviceRGB();
    CGFunctionRef functionObj = getFunction(colorspace);
    CGShadingRef shading = CGShadingCreateRadial(colorspace, CGPointMake(0.25, 0.3), 0.1, CGPointMake(0.7, 0.7), 0.25, functionObj, false, false);

    CGAffineTransform myTransform = CGAffineTransformMakeScale(100, 100);
    CGContextConcatCTM (context, myTransform);
    CGContextSaveGState (context);
    
    CGContextClipToRect (context, CGRectMake(0, 0, 1, 1));
    CGContextSetRGBFillColor (context, 1, 1, 1, 1);
    CGContextFillRect (context, CGRectMake(0, 0, 1, 1));
    
    CGContextDrawShading(context, shading);
    
    CGShadingRelease(shading);
    CGColorSpaceRelease(colorspace);
    CGFunctionRelease(functionObj);
    CGContextRestoreGState (context);
}

static CGFunctionRef getFunction(CGColorSpaceRef colorspace) {
    size_t numComponents = 1 + CGColorSpaceGetNumberOfComponents(colorspace);
    
    CGFloat input_value_range[] = {0, 1};
    CGFloat output_value_range[] = {0, 1, 0, 0, 0,0,1,1};
    
    CGFunctionCallbacks callbacks = {0, &calculateShadingValues, NULL};
    return CGFunctionCreate((void *) numComponents, 1, input_value_range, numComponents, output_value_range, &callbacks);

}

static void calculateShadingValues(void *info, const CGFloat *in, CGFloat *out) {
    
    size_t k, compoents;
    double frequency[] = {55, 220, 110, 0};
    compoents = (size_t)info;
    for (k = 0; k < compoents - 1; k ++)
        *out++ = (1 + sin(*in * frequency[k])) / 2;
    *out++ = 1;
}
```

![使用CGShading绘制径向渐变](http://oalg33nuc.bkt.clouddn.com/QQ20161221-3.png)

