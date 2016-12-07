iOS开发中，经常会遇到各种时间问题，网上资料大多零零散散。这里集中总结一下，以便于以后查阅和供大家参考。

## NSDate的8小时问题

### NSDate时间转为字符串时间

初始化一个`NSDate`时间`[NSDate date]`，获取的是零时区的时间（格林尼治的时间: 年-月-日 时:分:秒: +时区），而北京时间是东八区时间，因为时区不同，所以打印的时间相差了8小时。此刻表示的时间是一样的。

```Objective-C
NSDate *date = [NSDate date];
NSLog(@"date时间 = %@", date);

NSDateFormatter *formatter = [[NSDateFormatter alloc] init];
[formatter setDateFormat:@"yyyy-MM-dd HH:mm:ss"];
NSString *dateStr = [formatter stringFromDate:date];
NSLog(@"字符串时间 = %@", dateStr);
```

打印结果：

```Objective-C
2016-12-07 10:44:24.470 storyboardTest[32743:2995134] date时间 = 2016-12-07 02:44:24 +0000
2016-12-07 10:44:24.471 storyboardTest[32743:2995134] 字符串时间 = 2016-12-07 10:44:24
```

由打印结果前面的时间显示为准确地北京时间：`2016-12-07 10:44:24.470`。而`date`打印出来的时间少了8小时，因为它表示的是`零时区(+0000)`时间。此刻对应东八区的北京时间就是`10:44:24`。

系统会默认`[NSDate date]`获取的时间为零时区时间，而经过`NSDateFormatter`转化为字符串时间就是当前所在时区的准确时间，并没有8小时误差。

### 转化为字符串时间的时区设定

上文中NSDate时间转为字符串时间并没有设置`NSDateFormatter`的`timeZone`。不设置会默认使用当前所在的时区，与设置系统时区`formatter.timeZone = [NSTimeZone systemTimeZone]`的效果是一样的。

也可以设置时区，获取指定时区的字符串时间

```Objective-C
NSDate *date = [NSDate date];
NSDateFormatter *formatter = [[NSDateFormatter alloc] init];
[formatter setDateFormat:@"yyyy-MM-dd HH:mm:ss"];
formatter.timeZone = [NSTimeZone timeZoneWithName:@"Asia/Shanghai"];//东八区时间
NSString *dateStr = [formatter stringFromDate:date];
NSLog(@"字符串时间 = %@", dateStr);
```

这时获取的时间就是东八区时间，哪怕手机拿到零时区的格林尼治，获取的也是东八区的时间，因为这里指定时区了。也有如下时区指定：

```Objective-C
formatter.timeZone = [NSTimeZone timeZoneWithName:@"Asia/Tokyo"];//东九区时间
formatter.timeZone = [NSTimeZone timeZoneWithName:@"GMT"];//零区时间
```

通过下面方法可得到系统支持的时区对应的字符串常量：

```Objective-C
NSArray *zones = [NSTimeZone knownTimeZoneNames];
for (NSString *zone in zones) {
   NSLog(@"时区名 = %@", zone);
}
```

![时区对照表](https://github.com/mxdios/notebook/blob/master/notebooks/images/timezone.jpg?raw=true)

 
## 时间换算，时间戳的概念
## 时间格式
## 时间计算
## 获取周的问题


