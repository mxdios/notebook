# Quartz 2D

Quartz 2D是二维图形绘制引擎，可以实现N多图形图像的操作功能，如基本路径的绘制、透明度、描影、绘制阴影、透明层、颜色管理、反锯齿、PDF文档生成和PDF元数据访问。

## 画布Page

在图像操作过程中使用了`绘画者模型`，绘制过程是将绘制层铺到画布上，这个画布称为`Page`。类似于Photoshop中，将每个图层叠加放在画布上，形成最后的图像。

## 图形上下文Graphics Context

`图形上下文(Graphics Context)`是一个数据类型`CGContextRef`，它存放了Quartz 2D绘制的图形输出信息，可以看做是图形到设备输出的介质工具。Quartz 2D绘制的图形可以放到多种设备上，比如：PDF文件、显示器窗口、bitmap(位图)、view的Layer层等等，这就需要不同的`Graphics Context`来完成输出到不同设备上的工作。这好比人要上高速需要汽车，去海上需要船，去天空需要飞机，这里人相当于图形，高速路、海、天空相当于设备，而汽车、船、飞机就是`Graphics Context`。

`Graphics Context`的几种类型

```
1. Bitmap Graphics Context
2. PDF Graphics Context
3. Window Graphics Context
4. Layer Context
5. Post Graphics Context
```

## Quartz 2D的数据类型

Quartz 2D API属于Code Graphics框架，所以Quartz 2D的数据类型是以CG开头的。有以下数据类型：

```
1. CGPathRef：用于向量图，可创建路径，并进行填充或描画(stroke)
2. CGImageRef：用于表示bitmap图像和基于采样数据的bitmap图像遮罩。
3. CGLayerRef：用于表示可用于重复绘制(如背景)和幕后(offscreen)绘制的绘画层
4. CGPatternRef：用于重绘图
5. CGShadingRef、CGGradientRef：用于绘制渐变
6. CGFunctionRef：用于定义回调函数，该函数包含一个随机的浮点值参数。当为阴影创建渐变时使用该类型
7. CGColorRef, CGColorSpaceRef：用于告诉Quartz如何解释颜色
8. CGImageSourceRef,CGImageDestinationRef：用于在Quartz中移入移出数据
9. CGFontRef：用于绘制文本
10. CGPDFDictionaryRef, CGPDFObjectRef, CGPDFPageRef, CGPDFStream, CGPDFStringRef, and CGPDFArrayRef：用于访问PDF的元数据
11. CGPDFScannerRef, CGPDFContentStreamRef：用于解析PDF元数据
12. CGPSConverterRef：用于将PostScript转化成PDF。在iOS中不能使用。
```

## 图形状态

Quartz通过修改图形状态来修改绘制结果，图形状态直接决定了图形的最终渲染结果。图形状态包含用于绘制程序的参数，绘制参数改变了，图形自然就变了。比如修改了填充色值，图形颜色就变了。

## iOS中使用Graphics Context绘制图形

在iOS中要想用Quartz 2D在屏幕上绘图，需要自定义一个UIView，在UIView的`- (void)drawRect:(CGRect)rect`方法中实现绘图操作，这个方法会在UIView显示在屏幕上和需要被刷新的时候调用。创建上下文的方法如下：

```Objective-C
- (void)drawRect:(CGRect)rect {
    CGContextRef context = UIGraphicsGetCurrentContext();
}
```

在上下文中绘制图形：

```Objective-C
- (void)drawRect:(CGRect)rect {
    //获取图形上下文
    CGContextRef context = UIGraphicsGetCurrentContext();
    //设置红色透明度0.5的填充色
    CGContextSetRGBFillColor(context, 1, 0, 0, 0.5);
    //填充一个矩形frame为(10, 20, 100, 40)
    CGContextFillRect(context, CGRectMake(10, 20, 100, 40));
    //设置蓝色透明度0.5的填充色
    CGContextSetRGBFillColor(context, 0, 0, 1, 0.5);
    //填充一个矩形frame为(10, 20, 40,  100)
    CGContextFillRect(context, CGRectMake(10, 20, 40, 100));
}
```

![显示结果](https://github.com/mxdios/notebook/blob/master/notebooks/images/QQ20161209-0.png?raw=true)

注意：设置填充色和填充矩形的方法顺序不能颠倒，不然填充色填充不到想填充的矩形中。得不到填充色填充的会是黑色。

## 路径

路径可以构建出多种图形，可以是点、直线、弧线、不规则线、规则或不规则形状，可以对闭合路径进行填充行程面。使用路径绘制出想要的图形，有两步：创建路径和绘制路径。创建出点、线等路径，使用函数`CGContextDrawPath`绘制路径。

### 点

点是依靠x、y值固定的位置，可作为路径的起始点位置。比如要画一个线段，必须要有一个起点和一个终点，两点确定一条线段。使用函数`CGContextMoveToPoint`来确定起始点，传入图形上下文和x,y坐标点。

```Objective-C
//确定一个坐标为(10,10)的点
CGContextMoveToPoint(context, 10, 10);
```

### 直线

有起始点，再指定一个终点位置就能确定一条直线了，使用函数`CGContextAddLineToPoint`来指定终点位置。

```Objective-C
- (void)drawRect:(CGRect)rect {
	  //获取图形上下文
    CGContextRef context = UIGraphicsGetCurrentContext();
    //设置起点
    CGContextMoveToPoint(context, 20, 20);
    //起点延长至终点位置
    CGContextAddLineToPoint(context, 50, 50);
    //绘制路径
    CGContextDrawPath(context, kCGPathStroke);    
}
```

可以使用`CGContextAddLineToPoint`函数指定多个位置点，线段会一个接一个画下去，形成折线图形。也可以使用`CGContextAddLines`函数一次性指定多个位置点，完成折线图形，此时无需指定起始点，第一个点默认为起始点(即使用`CGContextMoveToPoint`指定起始点也无效)。

```Objective-C
//一个一个点继续加
CGContextMoveToPoint(context, 20, 20);    
CGContextAddLineToPoint(context, 50, 50);
CGContextAddLineToPoint(context, 30, 80);

//一次性指定多个点
CGPoint pos[3] = {CGPointMake(80, 20), CGPointMake(20, 50), CGPointMake(100, 200)};
CGContextAddLines(context, pos, 3);
```

由于`CGContextAddLineToPoint`函数必须得有`CGContextMoveToPoint`函数固定起始点，`CGContextAddLines`函数默认第一个点为起始点，所以`CGContextAddLines`后面可以跟着`CGContextAddLineToPoint`继续加点画线，能连成一组折线，而`CGContextMoveToPoint`+`CGContextAddLineToPoint`画线后面不能跟`CGContextAddLines`，会画成两组无关联的折线。

### 圆弧

画圆弧提供了两个函数，一个是`CGContextAddArc`，依次指定`图形上下文`、`圆心坐标`、`半径`、`起始弧度`、`终止弧度`、`画线顺时针(1)或逆时针(0)`。

```Objective-C
CGContextRef context = UIGraphicsGetCurrentContext();
CGContextAddArc(context, 100, 200, 30, 0, M_PI, 0);
CGContextDrawPath(context, kCGPathStroke);
```

第二个函数是`CGContextAddArcToPoint`，三点+半径确定一个圆弧。`CGContextMoveToPoint`函数确定第一个点，`CGContextAddArcToPoint`添加第二个点、第三个点和还有半径。原理：以第二点为中心，分别向第一点、第三点延长两条射线，射线夹角小于180°的一侧，以指定半径画圆弧，圆弧与射线相切。

```Objective-C
CGContextRef context = UIGraphicsGetCurrentContext();
CGContextMoveToPoint(context, 100, 100);
CGContextAddArcToPoint(context, 100, 150, 150, 150, 50);
CGContextDrawPath(context, kCGPathStroke);
```

### 曲线

画Bezier曲线也提供了两个函数，函数`CGContextAddCurveToPoint`用于画三次Bezier曲线，由一个起点、一个终点和两个控制点构成一条三次Bezier曲线。当两个控制点在起点和终点连线的同侧时，曲线会只有一个拱向，在不同侧时有两个拱向。

```Objective-C
CGContextRef context = UIGraphicsGetCurrentContext();
CGContextMoveToPoint(context, 100, 100);
CGContextAddCurveToPoint(context, 150, 50, 200, 200, 300, 100);
CGContextDrawPath(context, kCGPathStroke);
```

画二次Bezier曲线，使用函数CGContextAddQuadCurveToPoint。以当前点为起始点，指定一个控制点一个终点，确定一条二次Bezier曲线。控制点决定了曲线拱的方向，该函数只能创建一个拱向的Bezier曲线，而且曲线不可能交叉。

```Objective-C
CGContextRef context = UIGraphicsGetCurrentContext();
CGContextMoveToPoint(context, 100, 100);
CGContextAddQuadCurveToPoint(context, 200, 200, 300, 100);
CGContextDrawPath(context, kCGPathStroke);
```

### 闭合路径

Quartz 2D提供函数`CGContextClosePath`来闭合路径。就是将终点和起点用直线连接起来，使整个路径闭合。对于直线、弧、曲线等不能自动闭合的路径，如果想完成路径闭合必须调用该函数。

调用了`CGContextClosePath`完成闭合路径后，再调用`Add...`添加路径的函数，添加直线、弧线、曲线等路径，会从闭合路径的起始点开始。如果没有完成闭合路径函数的调用，则从终点继续开始新添加的路径。

```Objective-C
CGContextRef context = UIGraphicsGetCurrentContext();
CGContextMoveToPoint(context, 100, 100);
CGContextAddCurveToPoint(context, 150, 50, 150, 200, 300, 100);
CGContextClosePath(context);
CGContextAddLineToPoint(context, 200, 500);
CGContextDrawPath(context, kCGPathStroke);
```

### 矩形

使用函数`CGContextAddRect`来画矩形，函数中的`rect`中x,y是矩形左上角，宽高是矩形的宽高。

```Objective-C
CGContextRef context = UIGraphicsGetCurrentContext();
CGContextAddRect(context, CGRectMake(20, 20, 100, 40));
CGContextDrawPath(context, kCGPathStroke);
```

### 椭圆

使用函数`CGContextAddEllipseInRect`来画椭圆，是以矩形来确定椭圆。坐标参数意义和矩形一样，以确定的矩形内切画出的椭圆。矩形的圆心即是椭圆的圆心，矩形的宽高即是椭圆的长轴短轴，当矩形的宽高相等时，椭圆就是一个圆了。

```Objective-C
CGContextRef context = UIGraphicsGetCurrentContext();
CGContextAddEllipseInRect(context, CGRectMake(0, 0, 30, 50));
CGContextDrawPath(context, kCGPathStroke);
```

## 创建路径

-	在开始绘制路径前，调用 CGContextBeginPath 或 UI。 
-	直线、弧、曲线开始于当前点。空路径没有当前点；我们必须调用CGContextMoveToPoint来设置第一个子路径的起始点，或者调用一个便利函数来隐式地完成该任务。 
-	如果要闭合当前子路径，调用函数 CGContextClosePath。随后路径将开始一个新的子路径，即使我们不显示设置一个新的起始点。 
- 当绘制弧时，Quartz 将在当前点与弧的起始点间绘制一条直线。 
- 添加椭圆和矩形的 Quartz 程序将在路径中添加新的闭合子路径。 
-	我们必须调用绘制函数来填充或者描边一条路径，因为创建路径时并不会绘制路径。 
-	Quartz 提供了两个数据类型来创建可复用路径 CGPathRef 和 CGMutablePathRef。 
-	Quartz 提供了一个类似于操作图形上下文的 CGPath 的函数集合。这些路径函数操作 CGPath 对象，而不是图形上下文。 

```
1.	CGPathCreateMutable 取代 CGContextBeginPath
2.	CGPathMoveToPoint 取代 CGContextMoveToPoint
3.	CGPathAddLineToPoint 取代 CGContexAddLineToPoint
4.	CGPathAddCurveToPoint 取代 CGContexAddCurveToPoint
5.	CGPathAddEllipseInRect 取代 CGContexAddEllipseInRect
6.	CGPathAddArc 取代 CGContexAddArc
7.	CGPathAddRect 取代 CGContexAddRect
8.	CGPathCloseSubpath 取代 CGContexClosePath
```

如果想要添加一个路径到图形上下文，可以调用CGContextAddPath。路径将保留在图形上下文中，直到Quartz绘制它。我们可以调用CGContextAddPath再次添加路径。

## 绘制路径

通过图形上下文加上一系列创建路径的函数得到最终的路径，还需要绘制路径操作，才能使得路径呈现。如上文中一直用到的绘制路径的函数操作`CGContextDrawPath(context, kCGPathStroke);`。

绘制路径分为描边和填充，描边是绘制路径的边框，填充是绘制路径所包含的区域。

### 影响描边的属性

|函数|解释|备注|
|:---|:---|:---|
|CGContextSetLineWidth|描边路径的宽度|单位是用户空间单位|
|CGContextSetLineJoin|指定描边的连接点样式|枚举类型:Miter(尖角),Round(圆角),Bevel(平角)|
|CGContextSetLineCap|指定直线端点样式|枚举类:Butt,Round,Square|
|CGContextSetMiterLimit|转角的量级||
|CGContextSetLineDash|虚线模式|指定虚线数组和虚线相位控制虚线大小和位置|
|CGContextSetStrokeColorSpace|描边颜色空间|封装了颜色和颜色空间的CGColorRef|
|CGContextSetStrokeColor|设置描边颜色||
|CGContextSetStrokePattern|笔触模式||

### 填充路径

填充规则有两种：非零缠绕数规则（nonzero winding number rule）、偶数-奇数规则（even-odd rule）。

默认的填充规则为非零缠绕数规则。方法或枚举带有“EO”的为偶数-奇数规则。

非零缠绕数的填充规则与绘制的方向有关、偶数-奇数规则则与方向无关。如图。

![非零缠绕数规则和偶数奇数规则](http://oalg33nuc.bkt.clouddn.com/eosampleone.gif)

|函数|解释|
|:---|:---|
|CGContextEOFillPath|使用偶数奇数规则填充当前路径|
|CGContextFillPath|使用非零缠绕数规则填充当前路径|
|CGContextFillRect|快速填充一个矩形|
|CGContextFillRects|快速填充N个矩形|
|CGContextFillEllipseInRect|快速填充一个椭圆形|
|CGContextDrawPath|填充或描边路径|

CGContextDrawPath函数的使用：

- CGContextDrawPath(context, kCGPathFill) 填充路径。 
- CGContextDrawPath(context, kCGPathEOFill) 使用奇偶规则填充路径。 
- CGContextDrawPath(context, kCGPathStroke) 描边路径。 
- CGContextDrawPath(context, kCGPathFillStroke) 填充并描边路径。 
- CGContextDrawPath(context, kCGPathEOFillStroke) 使用奇偶规则填充并描边路径。 

### 混合模式

混合模式是Quartz将绘图绘制到背景上的方式，其实就是前景图和背景图怎么混合叠加。叠加公式如下：

```
result = (alpha * foreground) + (1 - alpha) *background
```

Quartz默认使用普通混合模式，也就是`kCGBlendModeNormal`。下面举例几个混合模式的样式，案例样式的代码见下一节`裁剪路径`：

|kCGBlendModeNormal|kCGBlendModeMultiply|kCGBlendModeScreen|
|:---:|:---:|:---:|
|![普通模式](http://oalg33nuc.bkt.clouddn.com/QQ20161213-0.png)|![正片叠底](http://oalg33nuc.bkt.clouddn.com/QQ20161213-1.png)|![屏幕混合](http://oalg33nuc.bkt.clouddn.com/QQ20161213-2.png)|

16种混合模式：

```Objective-C
kCGBlendModeNormal,			//普通
kCGBlendModeMultiply,		//正片叠底
kCGBlendModeScreen,			//屏幕
kCGBlendModeOverlay,		//叠加
kCGBlendModeDarken,			//暗化
kCGBlendModeLighten,		//亮化
kCGBlendModeColorDodge,		//色彩减淡
kCGBlendModeColorBurn,		//色彩加深
kCGBlendModeSoftLight,		//柔光
kCGBlendModeHardLight,		//强光
kCGBlendModeDifference,		//差值
kCGBlendModeExclusion,		//排除
kCGBlendModeHue,			//色相
kCGBlendModeSaturation,		//饱和度
kCGBlendModeColor,			//颜色
kCGBlendModeLuminosity,		//亮度
```

## 裁剪路径

裁剪是一个遮罩，会遮住不允许绘制的地方。裁剪区域是一个闭合路径，Quartz只会渲染裁剪区域里面的东西，外面的东西不渲染。通过函数`CGContextClip`裁剪，具体的裁剪代码和执行结果如下，注意代码位置：


```Objective-C
CGContextRef context = UIGraphicsGetCurrentContext();
/** 裁剪 */
CGContextBeginPath(context);
CGContextAddArc(context, 200, 200, 150, 0, 2 * M_PI, 0);
CGContextClosePath(context);
CGContextClip(context);

CGContextSetRGBFillColor(context, 0.5, 0.2, 1, 1);
CGContextFillRect(context, CGRectMake(100, 50, 50, 300));
CGContextSetRGBFillColor(context, 0.8, 0.3, 0.1, 1);
CGContextFillRect(context, CGRectMake(150, 50, 50, 300));
CGContextSetRGBFillColor(context, 0.5, 0.1, 0.4, 1);
CGContextFillRect(context, CGRectMake(200, 50, 50, 300));
CGContextSetRGBFillColor(context, 0, 0.5, 0.1, 1);
CGContextFillRect(context, CGRectMake(250, 50, 50, 300));
    
//设置混合模式，自行替换上述16中混合模式查看效果
CGContextSetBlendMode(context, kCGBlendModeMultiply); 

CGContextSetRGBFillColor(context, 0.5, 0.2, 1, 1);
CGContextFillRect(context, CGRectMake(20, 100, 350, 50));
CGContextSetRGBFillColor(context, 0.8, 0.3, 0.1, 1);
CGContextFillRect(context, CGRectMake(20, 150, 350, 50));
CGContextSetRGBFillColor(context, 0.5, 0.1, 0.4, 1);
CGContextFillRect(context, CGRectMake(20, 200, 350, 50));
CGContextSetRGBFillColor(context, 0, 0.5, 0.1, 1);
CGContextFillRect(context, CGRectMake(20, 250, 350, 50));
```

执行结果：

![裁剪路径和混合模式的执行结果](http://oalg33nuc.bkt.clouddn.com/QQ20161213-3.png)|

---

参考文章：[南峰子翻译的Quartz2D编程指南](http://southpeak.github.io/categories/translate/)


