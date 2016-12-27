## 透明层

透明层是将两个或两个以上的图形组合生成一个图形的方式。类似编辑ppt时，多个图形的组合操作。

1. 调用`CGContextBeginTransparencyLayer`函数开始透明层
2. 画要组合的各个图形
3. 调用`CGContextEndTransparencyLayer`函数结束透明层


```Objective-C
CGContextRef context = UIGraphicsGetCurrentContext();
CGContextSetShadow (context, CGSizeMake (20, 20), 5);
CGContextBeginTransparencyLayer (context, NULL);
CGContextSetRGBFillColor (context, 0, 1, 0, 1);
CGContextFillRect (context, CGRectMake(100, 100, 100, 100));
CGContextSetRGBFillColor (context, 0, 0, 1, 1);
CGContextFillRect (context, CGRectMake(130, 130, 100, 100));
CGContextSetRGBFillColor (context, 1, 0, 0, 1);
CGContextFillRect (context, CGRectMake(160, 160, 100, 100));
CGContextEndTransparencyLayer (context);
```

执行结果：

![透明层组合](http://oalg33nuc.bkt.clouddn.com/QQ20161227-0.png)

## 数据管理




