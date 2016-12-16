
## 颜色和颜色空间

不同设备的颜色处理方式各不相同，每种设备都有各自的颜色值范围。就是使用指定函数解析一组数值组成的颜色信息，比如RGB颜色、CMYK颜色。

在不同颜色空间下，蓝色的色值

|颜色空间|蓝色的色值|色值对应的参数含义|
|:---|:---|:---|
|HSB|240度，100%，100%|色相，饱和度，亮度|
|RGB|0，0，1|红，绿，蓝|
|CMYK|1，1，0，0|蓝绿、品红、黄、黑|
|BGR|1，0，0|蓝，绿，红|


## 透明度

使用函数`CGContextSetAlpha`设置全局透明度

```Objective-C
CGContextSetAlpha(context, 0.5);
```

使用函数`CGContextClearRect`清除图形上下文的alpha通道。在创建透明遮罩或者窗口完全透明的时候使用。

## 创建颜色空间

### 设备依赖颜色空间

**iOS不支持设备依赖颜色空间。**

1. lba非线性转换，使用`CGColorSpaceCreateLab`创建
2. ICC颜色空间，国际色彩联盟定义的，使用`CGColorSpaceCreateICCBased`创建
3. 标准化RGB颜色空间，使用`CGColorSpaceCreateCalibratedRGB`创建
4. 标准化灰度颜色空间，使用`CGColorSpaceCreateCalibratedGray`创建

### 通用颜色空间

**iOS不支持通用颜色空间。**

但是macOS应用程序通常情况下使用通用颜色空间，而不使用设备颜色空间。

使用函数`CGColorSpaceCreateWithName`创建通用颜色空间，传入固定常量：

1. `kCGColorSpaceGenericGray`，指定通用灰色
2. `kCGColorSpaceGenericRGB`，指定通用RGB
3. `kCGColorSpaceGenericCMYK`，指定通用CMYK

### 设备颜色空间

设备颜色空间主要用于IOS应用程序。通过以下函数创建设备颜色空间：

`CGColorSpaceCreateDeviceGray` 创建设备依赖灰度颜色空间。

快速设置依赖灰度颜色空间的方法：

```Objective-C
CGContextSetGrayStrokeColor(context, 0.5, 1);
CGContextSetGrayFillColor(context, 0.5, 1);
```

还可以使用该函数获取灰度图片，方法如下：

```Objective-C
- (UIImage *)getGrayImage:(UIImage*)sourceImage {
    CGFloat width = sourceImage.size.width;
    CGFloat height = sourceImage.size.height;
    CGColorSpaceRef colorSpace = CGColorSpaceCreateDeviceGray();
    CGContextRef context = CGBitmapContextCreate(nil,width,height,8,0,colorSpace,kCGImageAlphaNone);
    CGColorSpaceRelease(colorSpace);
    if (context == NULL) {
        return nil;
    }
    CGContextDrawImage(context,CGRectMake(0, 0, width, height), sourceImage.CGImage);
    UIImage *grayImage = [UIImage imageWithCGImage:CGBitmapContextCreateImage(context)];
    CGContextRelease(context);
    return grayImage;
}
```

![正常图片和灰度处理的图片](http://oalg33nuc.bkt.clouddn.com/QQ20161215-0.png)

`CGColorSpaceCreateDeviceRGB` 创建设备依赖RGB颜色空间，实现代码如下：

```Objective-C
CGContextRef context = UIGraphicsGetCurrentContext();
CGFloat colors[4] = {0.3, 1.0, 0.9, 1.0};
CGColorSpaceRef colorSpace = CGColorSpaceCreateDeviceRGB();
CGColorRef color = CGColorCreate(colorSpace, colors);
CGContextSetFillColorWithColor(context, color);
CGContextFillRect(context, CGRectMake(100, 50, 100, 50));
CGColorSpaceRelease(colorSpace);
CGColorRelease(color);
```

以及快速设置设备依赖RGB颜色空间的方法：

```Objective-C
CGContextSetRGBStrokeColor(context, 0.3, 1.0, 0.9, 1.0);
CGContextSetRGBFillColor(context, 0.3, 1.0, 0.9, 1.0);
```

![设备依赖RGB颜色空间](http://oalg33nuc.bkt.clouddn.com/QQ20161215-1.png)

`CGColorSpaceCreateDeviceCMYK` 创建设备依赖CMYK颜色空间，实现代码如下：

```Objective-C
CGContextRef context = UIGraphicsGetCurrentContext();
CGFloat colors[] = {80, 100, 0, 0, 1.0};
CGColorSpaceRef colorSpace = CGColorSpaceCreateDeviceCMYK();
CGColorRef color = CGColorCreate(colorSpace, colors);
CGContextSetFillColorWithColor(context, color);
CGContextFillRect(context, CGRectMake(100, 50, 100, 50));
CGColorSpaceRelease(colorSpace);
CGColorRelease(color);
```

以及快速设置设备依赖CMYK颜色空间的方法：

```Objective-C
CGContextSetCMYKStrokeColor(context, 80, 100, 0, 0, 1.0);
CGContextSetCMYKFillColor(context, 80, 100, 0, 0, 1.0);
```

![设备依赖CMYK颜色空间](http://oalg33nuc.bkt.clouddn.com/QQ20161215-2.png)


## 设置和创建颜色

通过函数`CGContextSetStrokeColorWithColor`和`CGContextSetFillColorWithColor`分别填充路径颜色和设置线框颜色。

```Objective-C
CGContextRef context = UIGraphicsGetCurrentContext();
CGContextSetLineWidth(context, 20);
CGContextAddRect(context, CGRectMake(100, 50, 100, 50));
CGContextSetStrokeColorWithColor(context, [UIColor blueColor].CGColor);
CGContextStrokePath(context);
CGContextSetFillColorWithColor(context, [UIColor redColor].CGColor);    
CGContextFillRect(context, CGRectMake(100, 50, 100, 50));
```

![填充路径设置线框](http://oalg33nuc.bkt.clouddn.com/QQ20161215-3.png)

## 设置再现意图

“再现意图”用于指定如何将源颜色空间的颜色映射到图形上下文的目标颜色空间的颜色范围内。调用函数`CGContextSetRenderingIntent`设置再现意图，传入枚举类型`CGColorRenderingIntent`

1.	kCGRenderingIntentDefault：使用默认的渲染意图。
2.	kCGRenderingIntentAbsoluteColorimetric：绝对色度渲染意图。将输出设备颜色域外的颜色映射为输出设备域内与之最接近的颜色。这可以产生一个裁减效果，因为色域外的两个不同的颜色值可能被映射为色域内的同一个颜色值。当图形使用的颜色值同时包含在源色域及目标色域内时，这种方法是最好的。常用于logo或者使用专色(spot color)时。
3.	kCGRenderingIntentRelativeColorimetric：相对色度渲染意图。转换所有的颜色(包括色域内的)，以补偿图形上下文的白点与输出设备白点之间的色差。kCGRenderingIntentPerceptual：感知渲染意图。通过压缩图形上下文的色域来适应输出设备的色域，并保持源颜色空间的颜色之间的相对性。感知渲染意图适用于相片及其它复杂的高细度图片。
4.	kCGRenderingIntentSaturation：饱和度渲染意图。把颜色转换到输出设备色域内时，保持颜色的相对饱和度。结果是包含亮度、饱和度颜色的图片。饱和度意图适用于生成低细度的图片，如描述性图表。

## 变换

变换是使用Quartz内置的变换函数对绘图进行平移、旋转和缩放变换。

## 修改CTM

CTM是Current Transformation Matrix，当前变换矩阵。可以操作CTM来平移、旋转、缩放page，绘制对象在page上，自然也会变换。

通过`CGContextDrawImage`函数绘制图片到page上。

```Objective-C
CGContextDrawImage(context, CGRectMake(20, 40, 100, 150), [UIImage imageNamed:@"img"].CGImage);
```

![将图片绘制到page上](http://oalg33nuc.bkt.clouddn.com/QQ20161215-4.png)

可以看到，图片是上下颠倒的。是因为iOS的UIKit坐标是以左上角为原点，y轴向下，Quartz 2D的图形绘制引擎坐标是左下角为原点，y轴向上。因为坐标轴上下相反，所以图片上下颠倒。macOS的坐标布局和Quartz 2D一样，以左下角为坐标原点，y轴向上。

注意要先进行CTM变换操作，再`CGContextDrawImage`画图。

### 平移

调用函数`CGContextTranslateCTM`指定沿x轴和y轴的平移位置。沿x轴平移100，沿y轴平移50：

```Objective-C
CGContextTranslateCTM(context, 100, 50);
```

### 旋转

调用函数`CGContextRotateCTM`指定旋转角度，是以坐标原点为中心，旋转指定角度。这个坐标原点是指iOS的UIKit的左上角坐标原点。

```Objective-C
CGContextRotateCTM(context, M_PI_4);
```

![旋转前与旋转后的图片比较](http://oalg33nuc.bkt.clouddn.com/QQ20161215-5.png)

函数传入的是弧度值，可以定义以下方法，传入角度值：

```Objective-C
static inline double radians (double degrees) {return degrees * M_PI/180;}
```

等价于上面的旋转函数调用：

```Objective-C
CGContextRotateCTM(context, radians(45.));
```

### 缩放

调用函数`CGContextScaleCTM`实现缩放操作，也是以坐标原点为中心，x，y，w，h皆缩放为指定比例

```Objective-C
CGContextScaleCTM(context, 0.5, 0.5);
```

![缩放前后的图片比较](http://oalg33nuc.bkt.clouddn.com/QQ20161215-6.png)

### 累计变换操作

上文说道，使用`CGContextDrawImage`函数画的图片上下颠倒，可以使用累计变换操作将图片扶正。累计变换操作的执行顺序不同，会导致不同结果。

```Objective-C
CGRect imgRect = CGRectMake(100, 40, 100, 130);
CGContextTranslateCTM(context, imgRect.size.width + imgRect.origin.x * 2, imgRect.size.height + imgRect.origin.y * 2);
CGContextRotateCTM(context, M_PI);
CGContextDrawImage(context, imgRect, [UIImage imageNamed:@"img"].CGImage);
```

![图片扶正结果](http://oalg33nuc.bkt.clouddn.com/QQ20161216-0.png)

## 仿射变换

仿射变换可以实现与CTM函数相同的变换操作，使用仿射变换函数构造矩阵，调用函数`CGContextConcatCTM`应用于CTM，达到变换效果。

仿射变换函数：

|函数|功能|
|:---|:---|
|CGAffineTransformMakeTranslation|构造平移矩阵，指定移动x,y值|
|CGAffineTransformTranslate|在现有的变换操作基础上使用平移操作|
|CGAffineTransformMakeRotation|构造旋转矩阵，指定旋转弧度|
|CGAffineTransformRotate|在现有的变换操作基础上使用旋转操作|
|CGAffineTransformMakeScale|构造缩放矩阵，指定x,y拉伸或收缩坐标|
|CGAffineTransformScale|在现有的变换操作基础上使用旋转操作|

构造变换矩阵的函数应用，执行结果和上文中图片一样，等价于上文中CTM函数变换操作：

```Objective-C
//平移
CGAffineTransform translation = CGAffineTransformMakeTranslation(100, 50);
CGContextConcatCTM(context, translation);
//旋转
CGAffineTransform rotation = CGAffineTransformMakeRotation(M_PI_4);
CGContextConcatCTM(context, rotation);
//缩放
CGAffineTransform scale = CGAffineTransformMakeScale(0.5, 0.5);
CGContextConcatCTM(context, scale);
```

在当前变换的基础上叠加变换操作，和上文中的累计变换操作等价：

```Objective-C
CGAffineTransform translate = CGAffineTransformTranslate(CGContextGetCTM(context), 100, 50);
CGAffineTransform rotate = CGAffineTransformRotate(translate, M_PI_4);
CGAffineTransform scale = CGAffineTransformScale(rotate, 0.5, 0.5);
CGContextConcatCTM(context, scale);
```


