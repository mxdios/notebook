# iOS多语言开发

好多主流app都支持多语言(或者叫本地化)，有的是自动适应系统语言，有的是用户选择修改。比如微信支持如下语言，可以根据系统自适应，也可以用户更改。

![img](https://github.com/mxdios/notebook/blob/master/notebooks/images/QQ20161117-0.png?raw=true)

## 创建多语言文件

在工程中`command + n`创建文件，选择`iOS` -> `Resource` -> `Strings File`文件，自定义文件名，创建完成。

## 添加多语言

选中新创建的多语言文件，展开右侧的侧边栏，在侧边栏里，点击`Localization`下的`Localize...`按钮，选择`English`，点击`Localize`。

选中工程在`PROJECT`里面的`Localizations`，添加语言。

每添加一个，在新建的多语言文件下就会多一个以`.strings`为后缀名的文件，这就是每个语言对应的语言文件。里面存放的语言文字是以`key - value`形式保存，每个文件中`key`是一样的，`value`对应着该语言下的文字。比如：

中文简体文件`File.strings(Chinese(Simplified))`里面存放的是：

```
"text"="世界，你好。";
```

英文文件`File.strings(English)`里存放的是：

```
"text"="hello,world.";
```

`key`值保持一致，是`text`，`value`对应该文字翻译的每种语言。

## 适应系统语言

访问语言文件的宏定义

```Objective-C
#define NSLocalizedString(key, comment) \
	    [NSBundle.mainBundle localizedStringForKey:(key) value:@"" table:nil]
#define NSLocalizedStringFromTable(key, tbl, comment) \
	    [NSBundle.mainBundle localizedStringForKey:(key) value:@"" table:(tbl)]
#define NSLocalizedStringFromTableInBundle(key, tbl, bundle, comment) \
	    [bundle localizedStringForKey:(key) value:@"" table:(tbl)]
#define NSLocalizedStringWithDefaultValue(key, tbl, bundle, val, comment) \
	    [bundle localizedStringForKey:(key) value:(val) table:(tbl)]
```

使用`NSLocalizedStringFromTable(key, tbl, comment)`宏定义赋值显示多语言的控件，`key`是多语言文件里的`key`值，`tbl`是多语言文件名，`comment`是注释，可空填nil。使用方式如下：

```Objective-C
_label.text = NSLocalizedStringFromTable(@"text", @"File", nil);
```

还可以使用宏`NSLocalizedString(key, comment)`，没有`tbl`文件名参数，这时工程里的多语言文件名必须为`Localizable`。

## 手动设定语言

语言文件在工程内是以`.lproj`格式存储的，比如简体中文是`zh-Hans.lproj`，繁体中文是`zh-Hant.lproj`，英文是`en.lproj`。

手动设定语言就是让工程访问哪个文件。要保留app的语言设置，下次打开app还是上次设置好的语言环境。语言设置属于偏好设置，一般用`NSUserDefaults`存储。

保存环境设置：

```Objective-C
NSUserDefaults *userd = [NSUserDefaults standardUserDefaults];
[userd setObject:@"zh-Hans" forKey:@"appLanguage"];//以简体中文为例
[userd synchronize];
```

设置语言环境：

```Objective-C
NSString *lanType =[NSString stringWithFormat:@"%@", [[NSUserDefaults standardUserDefaults] objectForKey:@"appLanguage"]];
NSString *path = [[NSBundle mainBundle] pathForResource:lanType ofType:@"lproj"];
NSString *showValue = [[NSBundle bundleWithPath:path] localizedStringForKey:@"showTxt" value:nil table:@"File"];
_label.text = showValue;
```

## 第一次打开app

### 手动设置默认语言

第一次打开app，本地UserDefaults没有存储语言信息，这时可以代码存入一个，后续即可正常访问。

在`didFinishLaunchingWithOptions`里简单判断一下即可。如果没有，即存入简体中文

```Objective-C
NSUserDefaults *userd = [NSUserDefaults standardUserDefaults];
if (![userd objectForKey:@"appLanguage"]) { 
     [userd setObject:@"zh-Hans" forKey:@"appLanguage"];
     [userd synchronize];
}
```

### 显示系统语言设置

首先要获取系统环境的语言，然后与app支持的语言做比较，如果app支持的语言里有目前系统语言，则显示该语言，如果没有则显示默认语言。

获取系统环境语言：

```Objective-C
NSString *language = [[NSLocale preferredLanguages] objectAtIndex:0];
```

具体实现：

```Objective-C
NSUserDefaults *userd = [NSUserDefaults standardUserDefaults];
if (![userd objectForKey:@"appLanguage"]) {//如果本地没有设置语言
    NSString *language = [[NSLocale preferredLanguages] objectAtIndex:0];//获取系统语言环境
    if ([language hasPrefix:@"zh-Hans"]) {//简体中文
        [userd setObject:@"zh-Hans" forKey:@"appLanguage"];//设置为简体中文
    } else if ([language hasPrefix:@"zh-TW"] || [language hasPrefix:@"zh-HK"] || [language hasPrefix:@"zh-Hant"]) {//台湾繁体，香港繁体和繁体
        [userd setObject:@"zh-Hant" forKey:@"appLanguage"];//设置为繁体
    } else if ([language hasPrefix:@"en"]) {//英文
        [userd setObject:@"en" forKey:@"appLanguage"];//设置为英文
    }else{//没有支持的语言
        [userd setObject:@"zh-Hans" forKey:@"appLanguage"];//设置为简体中文
    }
}
```


## 多语言开发的坑

`LaunchScreen`不支持多语言开发，苹果建议不要对启动页进行多语言开发。在`LaunchScreen.strings`文件中更改不同语言环境下的字符串，并没有效果。

解决办法：删除这些没用的`LaunchScreen.strings`文件

每种语言添加一个`LaunchScreen.storyboard`。比如英文对应：`LaunchScreen_en.storyboard`，繁体中文对应`LaunchScreen_zhHant.storyboard`。

在`InfoPlish.strings`下面对应的语言文件中，分别添加如下字段：

```
UILaunchStoryboardName = "LaunchScreen_en";//英文文件下添加的
```

其实这样做也有局限性，在首次安装启动时能选择正确的语言显示，手动更改手机语言环境，重新打开app则无法显示更改后的语言。—— 在模拟器上测试


