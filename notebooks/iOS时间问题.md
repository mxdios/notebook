在iOS开发中，经常会遇到各种各样的时间问题，8小时时差，时间戳，求时间间隔，农历等等。解决办法网上比比皆是，但大多零零散散，很多资料并没有说明其中问题。这里集中总结一下，以便于以后查阅和供大家参考。有我自己的理解，错漏之处请大家吐槽。

## NSDate的8小时问题

### NSDate转字符串时间

初始化一个`NSDate`时间`[NSDate date]`，获取的是零时区的时间（格林尼治的时间: 年-月-日 时:分:秒: +时区），而北京时间是东八区时间，因为时区不同，所以打印的时间相差了8小时。此刻表示的时间是一样的。

```Objective-C
NSDate *date = [NSDate date];
NSLog(@"date时间 = %@", date);

NSDateFormatter *formatter = [[NSDateFormatter alloc] init];
[formatter setDateFormat:@"yyyy-MM-dd HH:mm:ss Z"];
NSString *dateStr = [formatter stringFromDate:date];
NSLog(@"字符串时间 = %@", dateStr);
```

打印结果：

```Objective-C
2016-12-07 10:44:24.470 timeTest[32743:2995134] date时间 = 2016-12-07 02:44:24 +0000
2016-12-07 10:44:24.471 timeTest[32743:2995134] 字符串时间 = 2016-12-07 10:44:24 +0800
```

打印结果前面的时间是北京时间：`2016-12-07 10:44:24.470`。而`date`打印出来的时间显示少了8小时，因为它表示的是`零时区(+0000)`时间`02:44:24`。此刻对应东八区的北京时间就是`10:44:24`。只是时区不同，表示的时间点是一样的。好比1公斤和2斤，重量是一样的。`[NSDate date]`获取的时间单位是`零时区(+0000)`，我们所要的北京时间的单位是`东八区(+0800)`。

系统会默认`[NSDate date]`获取的时间为零时区时间，而经过`NSDateFormatter`转化为字符串时间就是当前所在时区的准确时间，并没有8小时误差。

### 转字符串时间的时区设定

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

### 字符串时间转NSDate

字符串时间转为NSDate时间也会有时区问题。也会遇到有所谓的8小时误差，其实就是时区不同。比如下面的例子：

```Objective-C
NSDateFormatter *formatter = [[NSDateFormatter alloc] init];
[formatter setDateFormat:@"yyyy-MM-dd HH:mm:ss Z"];
NSDate *newDate = [formatter dateFromString:@"2016-12-07 14:06:24 +0800"];
NSLog(@"newDate = %@", newDate);
```

打印结果：

```Objective-C
2016-12-07 14:12:17.468 timeTest[34279:3155380] newDate = 2016-12-07 06:06:24 +0000
```

`NSDateFormatter`的指定格式是：`@"yyyy-MM-dd HH:mm:ss Z"`。这里面的`Z`指的是时区。要转化的字符串时间格式必须和这个格式匹配，上面给定的字符串时间是：`@"2016-12-07 14:06:24 +0800"`，是一个东八区时间，转化为NSDate后是零区时间`2016-12-07 06:06:24 +0000`，字面显示上少了8小时，其实时间一样。

其实如果上面给定的字符串时间为`@"2016-12-07 14:06:24 +0000"`，转化出来的NSDate时间会完全一样，因为字符串时间为零时区时间，不存在时区误差。大家可以试一下。

当不指定字符串时间的时区时，即没有后面的`+0800`，同时要把`NSDateFormatter`时间格式里的`Z`去掉，保证格式匹配。系统会认为字符串时间是系统所在时区的时间，转化为NSDate时间是零时区时间。

同样，也可以使用`formatter.timeZone = [NSTimeZone timeZoneWithName:@"GMT"];`这种方式指定字符串时间的时区，和用`Z`对应`+0000`是一样的。

### NSDate转当前时区的NSDate时间

因为`[NSDate date]`得出的时间是零时区时间，当我们要获取当前所在时区的NSDate时间时，通常会用以下方法：

```Objective-C
NSDate *date = [NSDate date];
NSTimeZone *zone = [NSTimeZone systemTimeZone];
NSInteger interval = [zone secondsFromGMTForDate:date];
NSDate *localDate = [date  dateByAddingTimeInterval:interval];
NSLog(@"localDate = %@",localDate);
```

打印结果：

```Objective-C
2016-12-07 14:49:03.777 timeTest[34519:3183548] localDate = 2016-12-07 14:49:03 +0000
```

上面代码中`zone`是当前时区，`interval`是当前时区和零时区时间的差值，最后结果`localDate`是零时区时间`date`加上这个差值`interval`，得到当前时区的NSDate时间。更有甚者，在开发中直接加`8*60*60`或`28800`这样的值，因为相差8小时嘛。这样在东八区没问题，在其他时区时间就错了。

其实这种做法是不科学的，因为得到的最终时间还是零时区时间，时间后面明显是`+0000`，在使用中一般不显示时区，所以认为当做当前时区的时间使用也未尝不可。此为大坑！

坑1：这时如果转为字符串时间，又会增加8小时。因为做时间转换的时候，系统会认为这个NSDate是零时区，得到的字符串时间是东八区的。

解决办法是：将错就错，字符串时间也设置为零时区的字符串时间。从深坑跌入更深的坑！

```Objective-C
NSDate *date = [NSDate date];
NSTimeZone *zone = [NSTimeZone systemTimeZone];
NSInteger interval = [zone secondsFromGMTForDate:date];
NSDate *localDate = [date dateByAddingTimeInterval:interval];

NSDateFormatter *formatter = [[NSDateFormatter alloc] init];
[formatter setDateFormat:@"yyyy-MM-dd HH:mm:ss"];
formatter.timeZone = [NSTimeZone timeZoneWithName:@"UTC"];
NSString *dateStr = [formatter stringFromDate:localDate];
NSLog(@"字符串时间 = %@", dateStr);
```

这里的`@"UTC"`是指世界标准时间，也是现在用的时间标准，东八区比这个时间也是快8小时，这里填`@"GMT"`也是可以的。

坑2：在与后台交互时，有时需要`+0000`时区，这时只能手动拼接字符串更改这个时区字段，改为正确的时区。

**所以，在开发中尽量不要这么做，当时间要求显示、存储或与后台交互的时候，使用字符串时间！不要使用转化的NSDate。**

## 时间换算，时间戳的概念

### 当前时间转时间戳

时间戳是指1970年1月1日0时0分0秒到当前时间的秒数。注意：这里的当前时间是指零时区的NSDate时间。

```Objective-C
NSDate *date = [NSDate date];
NSTimeInterval timeIn = [date timeIntervalSince1970];
NSLog(@"时间戳 = %.0f", timeIn);
```

打印结果：

```Objective-C
2016-12-07 15:41:04.000 timeTest[34994:3232390] 时间戳 = 1481096464
```

### 时间戳转当前时间

```Objective-C
NSDate *date = [NSDate date];
NSTimeInterval timeIn = [date timeIntervalSince1970];
NSDate *newDate = [NSDate dateWithTimeIntervalSince1970:timeIn];
NSDateFormatter *dateFormatter = [[NSDateFormatter alloc] init];
[dateFormatter setDateFormat:@"yyyy-MM-dd HH:mm:ss Z"];
NSString *newTime = [dateFormatter stringFromDate:newDate];
NSLog(@"初始化时间 = %@，时间戳=%.0f，时间戳转为NSDate时间 = %@，转为字符串时间 = %@", date, timeIn, newDate, newTime);
```

打印结果：

```Objective-C
2016-12-07 16:11:56.146 timeTest[35186:3253589] 初始化时间 = 2016-12-07 08:11:56 +0000，时间戳=1481098316，时间戳转为NSDate时间 = 2016-12-07 08:11:56 +0000，转为字符串时间 = 2016-12-07 16:11:56 +0800
```

注意时间戳使用的NSDate时间是当前零时区的时间！当前零时区时间！当前零时区时间！重要的事情说三遍！不要进行NSDate转当前时区的NSDate时间，再转时间戳。下面是验证：

```Objective-C
NSDate *date = [NSDate date];
NSLog(@"系统零时区NSDate时间 = %@", date);    
NSTimeInterval timeIn = [date timeIntervalSince1970];
NSLog(@"系统零时区NSDate时间转化为时间戳 = %.0f", timeIn);
NSTimeZone *zone = [NSTimeZone systemTimeZone];
NSInteger interval = [zone secondsFromGMTForDate:date];
NSDate *localDate = [date  dateByAddingTimeInterval:interval];
NSLog(@"转化为本地NSDate时间 = %@", localDate);
NSTimeInterval timeIn2 = [localDate timeIntervalSince1970];
NSLog(@"本地NSDate时间转化为时间戳 = %.0f", timeIn2);
NSDate *detaildate = [NSDate dateWithTimeIntervalSince1970:timeIn];
NSDate *detaildate2 = [NSDate dateWithTimeIntervalSince1970:timeIn2];
NSDateFormatter *dateFormatter = [[NSDateFormatter alloc] init];
[dateFormatter setDateFormat:@"yyyy-MM-dd HH:mm:ss Z"];
NSString *newTime = [dateFormatter stringFromDate:detaildate];
NSString *newTime2 = [dateFormatter stringFromDate:detaildate2];
NSLog(@"最终转为字符串时间1 = %@， 时间2 = %@", newTime, newTime2);
```

打印结果：

```Objective-C
2016-12-07 16:13:57.834 timeTest[35211:3255842] 系统零时区NSDate时间 = 2016-12-07 08:13:57 +0000
2016-12-07 16:13:57.834 timeTest[35211:3255842] 系统零时区NSDate时间转化为时间戳 = 1481098438
2016-12-07 16:13:57.835 timeTest[35211:3255842] 转化为本地NSDate时间 = 2016-12-07 16:13:57 +0000
2016-12-07 16:13:57.835 timeTest[35211:3255842] 本地NSDate时间转化为时间戳 = 1481127238
2016-12-07 16:13:57.836 timeTest[35211:3255842] 最终转为字符串时间1 = 2016-12-07 16:13:57 +0800， 时间2 = 2016-12-08 00:13:57 +0800
```

问题解释详见上文的`NSDate转当前时区的NSDate时间`。

## 时间操作与比较

### 时间初始化和比较方法

几个时间初始化方法：

```Objective-C
//初始化当前时间，返回零时区时间
NSDate *date = [NSDate date];
//以当前时间为准，正数超前指定秒数，负数延后指定秒数
NSDate *laterDate = [NSDate dateWithTimeIntervalSinceNow:60];
//以2001-01-01 00:00:00 +0000为基准，正数超前指定秒数，负数延后指定秒数
NSDate *newDate = [NSDate dateWithTimeIntervalSinceReferenceDate:60];
//以1970-01-01 00:00:00 +0000为基准，正数超前指定秒数，负数延后指定秒数
NSDate *newDate1 = [NSDate dateWithTimeIntervalSince1970:60];
//实例方法，以指定时间为基准，正数超前指定秒数，负数延后指定秒数
NSDate *newDate2 = [date dateByAddingTimeInterval:60];
//很久以后的某一天
NSDate *newDate3 = [NSDate distantFuture];
//很久以前的某一天
NSDate *newDate4 = [NSDate distantPast];
```

几个时间比较方法：

```Objective-C
//比较两个时间是否相等
- (BOOL)isEqualToDate:(NSDate *)otherDate;
//两个时间比较，返回较早时间
- (NSDate *)earlierDate:(NSDate *)anotherDate;
//两个时间比较，返回较晚时间
- (NSDate *)laterDate:(NSDate *)anotherDate;
//两个时间比较，返回枚举类型
- (NSComparisonResult)compare:(NSDate *)other;
```

几个计算时间间隔的方法：

```Objective-C
//返回实例时间与refDate时间间隔秒数
- (NSTimeInterval)timeIntervalSinceDate:(NSDate *)refDate;
//返回实例时间与当前时间间隔秒数
- (NSTimeInterval)timeIntervalSinceNow;
//返回实例时间的时间戳
- (NSTimeInterval)timeIntervalSince1970;
//返回实例时间和2001-01-01 00:00:00 +0000的间隔秒数
- (NSTimeInterval)timeIntervalSinceReferenceDate;
//返回当前时间和2001-01-01 00:00:00 +0000的间隔秒数
+ (NSTimeInterval)timeIntervalSinceReferenceDate;
```

### 获取年月日时分秒周时区

oc里的时间坑太多，根本没办法像其他语言那样直接time.year就能获取年份。要想获取NSDate的年月日需要使用日历对象`NSCalendar`。

```Objective-C
NSDate *date = [NSDate date];
NSCalendar *cal = [NSCalendar currentCalendar];
NSDateComponents *dateComps = [cal components:NSCalendarUnitYear|NSCalendarUnitMonth|NSCalendarUnitDay|NSCalendarUnitHour|NSCalendarUnitMinute|NSCalendarUnitSecond|NSCalendarUnitWeekday|NSCalendarUnitWeekOfMonth|NSCalendarUnitWeekOfYear|NSCalendarUnitTimeZone fromDate:date];
NSLog(@"时间 = %@", date);
NSLog(@"年=%ld,月=%ld,日=%ld,时=%ld,分=%ld,秒=%ld,周=%ld,本月第%ld周,本年第%ld周,时区=%@", dateComps.year, dateComps.month, dateComps.day, dateComps.hour, dateComps.minute, dateComps.second, dateComps.weekday, dateComps.weekOfMonth, dateComps.weekOfYear, dateComps.timeZone.name);
```

打印结果：

```Objective-C
2016-12-07 17:20:41.639 timeTest[35734:3311752] 时间 = 2016-12-07 09:20:41 +0000
2016-12-07 17:20:41.640 timeTest[35734:3311752] 年=2016,月=12,日=7,时=17,分=20,秒=41,周=4,本月第2周,本年第50周,时区=Asia/Shanghai
```

`NSDateComponents`创建方法中添加的枚举`NSCalendarUnit`，是后面要获取的年月日时分秒必须对应添加的。比如要获取年`dateComps.year`，就需要添加枚举`NSCalendarUnitYear`。

可以看到，`[NSDate date]`时间可以使用`NSCalendar`直接获取当前时区的时分秒，打印的`时`和`时区`即可看出。这是`[NSCalendar currentCalendar]`日历对象初始化的原因，也可以用`[[NSCalendar alloc] initWithCalendarIdentifier:NSCalendarIdentifierGregorian]`指定Identifier的方式初始化阳历日历。可以试试指定Identifier为`NSCalendarIdentifierChinese`，打印的是中国农历。

`dateComps.weekOfMonth`是今天属于本月的第几周。

`dateComps.weekOfYear`是今天属于本年的第几周。

`dateComps.weekday`是星期，这个和日常使用有些不同。上述程序打印的是`周=4`，但2016-12-07是`周三`。这里`weekday`的对应关系是：周日-1，周一-2，周二-3，周三-4，周四-5，周五-6，周六-7。毕竟国外惯例周日是每周的第一天。

## 农历

获取农历的工具方法，可根据需求添加农历节日和二十四节气

```Objective-C
+ (NSString *)LunarForSolarYear:(int)wCurYear Month:(int)wCurMonth Day:(int)wCurDay
{
    //农历日期名
    NSArray *cDayName =  [NSArray arrayWithObjects:@"*",@"初一",@"初二",@"初三",@"初四",@"初五",@"初六",@"初七",@"初八",@"初九",@"初十",@"十一",@"十二",@"十三",@"十四",@"十五",@"十六",@"十七",@"十八",@"十九",@"二十",@"廿一",@"廿二",@"廿三",@"廿四",@"廿五",@"廿六",@"廿七",@"廿八",@"廿九",@"三十",nil];
    //农历月份名
    NSArray *cMonName =  [NSArray arrayWithObjects:@"*",@"正月",@"二月",@"三月",@"四月",@"五月",@"六月",@"七月",@"八月",@"九月",@"十月",@"冬月",@"腊月",nil];
    //公历每月前面的天数
    const int wMonthAdd[12] = {0,31,59,90,120,151,181,212,243,273,304,334};
    //农历数据
    const int wNongliData[100] = {2635,333387,1701,1748,267701,694,2391,133423,1175,396438
        ,3402,3749,331177,1453,694,201326,2350,465197,3221,3402
        ,400202,2901,1386,267611,605,2349,137515,2709,464533,1738
        ,2901,330421,1242,2651,199255,1323,529706,3733,1706,398762
        ,2741,1206,267438,2647,1318,204070,3477,461653,1386,2413
        ,330077,1197,2637,268877,3365,531109,2900,2922,398042,2395
        ,1179,267415,2635,661067,1701,1748,398772,2742,2391,330031
        ,1175,1611,200010,3749,527717,1452,2742,332397,2350,3222
        ,268949,3402,3493,133973,1386,464219,605,2349,334123,2709
        ,2890,267946,2773,592565,1210,2651,395863,1323,2707,265877};
    static int nTheDate,nIsEnd,m,k,n,i,nBit;
    //计算到初始时间1921年2月8日的天数：1921-2-8(正月初一)
    nTheDate = (wCurYear - 1921) * 365 + (wCurYear - 1921) / 4 + wCurDay + wMonthAdd[wCurMonth - 1] - 38;
    if((!(wCurYear % 4)) && (wCurMonth > 2))
        nTheDate = nTheDate + 1;
    //计算农历天干、地支、月、日
    nIsEnd = 0;
    m = 0;
    while(nIsEnd != 1) {
        if(wNongliData[m] < 4095)
            k = 11;
        else
            k = 12;
        n = k;
        while(n>=0) {
            //获取wNongliData(m)的第n个二进制位的值
            nBit = wNongliData[m];
            for(i=1;i<n+1;i++)
                nBit = nBit/2;
            nBit = nBit % 2;
            
            if (nTheDate <= (29 + nBit)) {
                nIsEnd = 1;
                break;
            }
            nTheDate = nTheDate - 29 - nBit;
            n = n - 1;
        }
        if(nIsEnd)
            break;
        m = m + 1;
    }
    wCurYear = 1921 + m;
    wCurMonth = k - n + 1;
    wCurDay = nTheDate;
    if (k == 12) {
        if (wCurMonth == wNongliData[m] / 65536 + 1)
            wCurMonth = 1 - wCurMonth;
        else if (wCurMonth > wNongliData[m] / 65536 + 1)
            wCurMonth = wCurMonth - 1;
    }
    //生成农历月
    NSString *szNongliMonth;
    if (wCurMonth < 1) {
        szNongliMonth = [NSString stringWithFormat:@"闰%@",(NSString *)[cMonName objectAtIndex:-1 * wCurMonth]];
    }else{
        szNongliMonth = (NSString *)[cMonName objectAtIndex:wCurMonth];
    }
    //生成农历日
    NSString *szNongliDay = [cDayName objectAtIndex:wCurDay];
    //合并
    NSString *lunarDate = [NSString stringWithFormat:@"%@-%@",szNongliMonth,szNongliDay];
    return lunarDate;
}
```

