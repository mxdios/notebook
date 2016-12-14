
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

1. `CGColorSpaceCreateDeviceGray` 创建设备依赖灰度颜色空间
2. `CGColorSpaceCreateDeviceRGB` 创建设备依赖RGB颜色空间
3. `CGColorSpaceCreateDeviceCMYK` 创建设备依赖CMYK颜色空间





```Objective-C
CGContextSetRGBFillColor(context, 0.5, 0.2, 1, 1);
CGContextSetCMYKFillColor(context, 0.5, 0.1, 0.9, 0.3, 1);
```


iOS不支持设备依赖颜色空间或通用颜色空间。iOS应用程序必须使用设备颜色空间(device color space)。


