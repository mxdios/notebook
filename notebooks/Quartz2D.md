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

## 图形上下文Graphics Context的创建应用

### iOS中使用Graphics Context绘制图形

在iOS中要想用Quartz 2D在屏幕上绘图，需要自定义一个UIView，在UIView的`- (void)drawRect:(CGRect)rect`方法中实现绘图操作，这个方法会在UIView显示在屏幕上和需要被刷新的时候调用。创建上下文的方法如下：

```
- (void)drawRect:(CGRect)rect {
    CGContextRef context = UIGraphicsGetCurrentContext();
}
```

