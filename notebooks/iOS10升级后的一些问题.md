# iOS10升级

在2016年9月14日凌晨iOS10系统准时推送过来了，作为懒人之前的beta版一直没有升级试用。今天果断手机和Xcode一起升级了，新系统新面貌带来新bug。记录一下升级iOS10和Xcode8.0后遇到的问题。

# 开发者证书不能使用

General里面Signing变成如下样子，Automatically manage signing 自动签名管理。报错了，之前一直使用的企业级开发者证书不可用了。

![img](https://github.com/mxdios/notebook/blob/master/notebooks/images/WechatIMG79.jpeg?raw=true)

Build Settings 里面的 Code Signing Identity 清一色选择为企业级开发者证书。还是报错

![img](https://github.com/mxdios/notebook/blob/master/notebooks/images/QQ20160914-0.png?raw=true)

![img](https://github.com/mxdios/notebook/blob/master/notebooks/images/QQ20160914-1.png?raw=true)

**解决办法：**

取消Automatically manage signing的选择，会出现两个Signing，分别对应Debug和Release，选择该项目对应使用的Provisioning Profiles即可

![img](https://github.com/mxdios/notebook/blob/master/notebooks/images/QQ20160914-2.png?raw=true)

# 注释快捷键command+/失效

Xcode8.0的注释// 生成快捷键 command+/失效了

**解决办法：**

终端运行命令：`sudo /usr/libexec/xpccachectl`
Password：`输入开机密码`
重启Mac生效

生效后空行快捷键，只在代码行能产生/解除注释，空行不能产生注释。记得之前可以来这....

# Xcode控制台输出问题

## 无用log打印

升级Xcode8.0之后运行项目，控制台疯狂打印了N多东西，完全看不懂啊，完犊子了，这么多需要适配的...

眼不见心不烦，下面就提供去掉这些乱七八糟打印内容的方法。

**解决办法：**

在`Edit Scheme...`下的Run -> Arguments -> Environment Variables 添加  `OS_ACTIVITY_MODE` = `disable`

具体见下图

![img](https://github.com/mxdios/notebook/blob/master/notebooks/images/QQ20160914-3.png?raw=true)

## 真机测试log屏蔽

经过上述修改，在iOS10模拟器中无用的log输出被屏蔽了，但是真机测试的时候，没有log输出，log日志被完全屏蔽了。不知是屏蔽无用log的设置，还是xcode8为了提高真机测试性能屏蔽了log日志？真机测试的log也很重要。

为了app正式发布时不打印log，让打印操作`NSLog`只在`Debug`环境下运行，采用了下面的办法自定义`NSLog`:

```Objective-c
#ifdef DEBUG
#define XDLog(...) NSLog(__VA_ARGS__)
#else
#define XDLog(...)
#endif
```
iOS10之后，在真机中`NSLog`无法打印log，可以使用`printf()`，具体修改见下面自定义log：

```Objective-c
#ifdef DEBUG
#define XDString [NSString stringWithFormat:@"%s", __FILE__].lastPathComponent
#define XDLog(...) printf("%s %d行: %s\n\n",[XDString UTF8String], __LINE__, [[NSString stringWithFormat:__VA_ARGS__] UTF8String]);
#else
#define XDLog(...)
#endif
```
`XDString`是获取的该log在哪个文件里，`__LINE__`该log在第几行。`printf()`是c语言方法，所以要用`UTF8String`转义，不然每个log都会有警告。


#  类型判断

网络获取数据，做容错处理，判断某个字段是否符合需要的数据类型。之前一度用如下判断方法：

```Objective-c
[NSStringFromClass([userDict[@"plates"] class]) isEqualToString:@"__NSCFArray"]
```
如果`userDict[@"plates"]`的数据类型是数组的话，打印出来的就是`__NSCFArray`类型。同样如果是字典类型的话，打印出来的就是`__NSCFDictionary`。但是iOS10之后，打印出来的不同了，同样的数组类型，打印出来的是`__NSSingleObjectArrayI`。

之前的这种判断方法很欠考虑，还是利用下面的方法比较靠谱：

```Objective-c
[userDict[@"plates"] isKindOfClass:[NSArray class]]
```

# URL Schemes跳转系统设置

跳转系统设置的方法彻底被关闭了，也就是说你已不能从app跳转到系统设置里了。我的某个app使用地理定位，运行app时会检测是否开启地理定位，如果未开启，提醒alert，用如下代码点击跳转到地理定位的开启设置里。

```Objective-c
NSURL *url = [NSURL URLWithString:@"prefs:root=LOCATION_SERVICES"];
if ([[UIApplication sharedApplication] canOpenURL:url]) {
  [[UIApplication sharedApplication] openURL:url];
}
```
iOS10以后`prefs:root`开头的Scheme无法跳转到系统设置里了。

# plist里声明获取隐私数据权限

访问隐私数据需要在plist里声明，在iOS10之前只需要声明地理定位之类的敏感隐私数据，获取照片、相机等不需要再plist里声明。iOS10之后，这些也必须声明，不声明会crash。需要声明的用户数据有：

> Contacts（联系人）, Calendar（日历）, Reminders（提醒事件）, Photos（照片）, Bluetooth Sharing（蓝牙共享）, Microphone（麦克风）, Camera（相机）, Location（位置）, Health（健康）, HomeKit（家居）, Media Library（媒体库）, Motion（运动）, CallKit（打电话）, Speech Recognition（语言识别）, SiriKit（Siri）, TV Provider（电视提供商）.

![img](https://github.com/mxdios/notebook/blob/master/notebooks/images/QQ20160921-0.png?raw=true)

后面string字段填写弹出用户允许时展示的描述信息。**注意，这里必须要写明获取该权限的用途，不然会被AppStore拒绝上架的。**

# 某些app链接网络失败

iOS10之后出现了实用无线局域网与蜂窝移动网络的允许授权弹窗，有些应用没有出现这个弹窗，莫名其妙的就连不上网络了，去设置的“使用无线局域网与蜂窝移动的应用”里找也找不到，搜索该应用，也没有开启使用网络授权的开关。其实只需要在“使用无线局域网与蜂窝移动的应用”里面修改任意一个应用的使用网络设置，再打开连不上网的应用，就会出现网络授权弹窗。这是一个系统bug，希望后续的iOS10.x.x系统会修复。


