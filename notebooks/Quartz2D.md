# Quartz 2D

Quartz 2D是二维图形绘制引擎，可以实现N多图形图像的操作功能，如基本路径的绘制、透明度、描影、绘制阴影、透明层、颜色管理、反锯齿、PDF文档生成和PDF元数据访问。

## 画布Page

在图像操作过程中使用了`绘画者模型`，绘制过程是将绘制层铺到画布上，这个画布称为`Page`。类似于Photoshop中，将每个图层叠加放在画布上，形成最后的图像。

## 图形上下文Graphics Context

`图形上下文(Graphics Context)`是一个数据类型`CGContextRef`，它存放了Quartz 2D绘制的图形输出信息，可以看做是图形到设备输出的介质工具。Quartz 2D绘制的图形可以放到多种设备上，比如：PDF文件、显示器窗口、bitmap(位图)、view的Layer层等等，这就需要不同的`Graphics Context`来完成输出到不同设备上的工作。这好比人要上高速需要汽车，去海上需要船，去天空需要飞机，这里人相当于图形，高速路、海、天空相当于设备，而汽车、船、飞机就是`Graphics Context`。

`Graphics Context`的几种类型

```
1.	Bitmap Graphics Context
2.	PDF Graphics Context
3.	Window Graphics Context
4.	Layer Context
5.	Post Graphics Context
```

## Quartz 2D的数据类型

Quartz 2D API属于Code Graphics框架，所以Quartz 2D的数据类型是以CG开头的。有以下数据类型：

```
1.	CGPathRef：用于向量图，可创建路径，并进行填充或描画(stroke)
2.	CGImageRef：用于表示bitmap图像和基于采样数据的bitmap图像遮罩。
3.	CGLayerRef：用于表示可用于重复绘制(如背景)和幕后(offscreen)绘制的绘画层
4.	CGPatternRef：用于重绘图
5.	CGShadingRef、CGGradientRef：用于绘制渐变
6.	CGFunctionRef：用于定义回调函数，该函数包含一个随机的浮点值参数。当为阴影创建渐变时使用该类型
7.	CGColorRef, CGColorSpaceRef：用于告诉Quartz如何解释颜色
8.	CGImageSourceRef,CGImageDestinationRef：用于在Quartz中移入移出数据
9.	CGFontRef：用于绘制文本
10.	CGPDFDictionaryRef, CGPDFObjectRef, CGPDFPageRef, CGPDFStream, CGPDFStringRef, and CGPDFArrayRef：用于访问PDF的元数据
11.	CGPDFScannerRef, CGPDFContentStreamRef：用于解析PDF元数据
12.	CGPSConverterRef：用于将PostScript转化成PDF。在iOS中不能使用。
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



```Objective-C
CGContextRef context = UIGraphicsGetCurrentContext();
CGContextMoveToPoint(context, 100, 100);
CGContextAddCurveToPoint(context, 150, 50, 200, 200, 300, 100);
CGContextDrawPath(context, kCGPathStroke);
```

