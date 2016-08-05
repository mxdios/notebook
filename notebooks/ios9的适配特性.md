# iOS9的适配特性

[参考文章内容](https://github.com/ChenYilong/iOS9AdaptationTips#certificate-transparency)

## 安全的https请求

iOS9默认把http请求，改为https请求。afn底层应用NSURLConnection受到影响。这一做法称为ATS, App Transport Security 应用程序的安全传输

**TLS1.0 == SSL3.1**


HTTP+SSL/TLS+TCP = HTTPS

### 为了访问http，在info.plist加入以下字段，退回到不安全的网络请求。

App Transport Security Settings

Allow Arbitrary Loads

NSAppTransportSecurity

NSAllowArbitraryLoads

### 新特性的解释

xcode7在编辑app的时候，iOS9下不能进行网络访问。如果从AppStore上下载app，还能正常进行网络访问。

因为ATS是在iOS9.X-SDK下的新特性，AppStore上的应用是基于iOS8.x-SDK或者iOS7.X-SDK的，所以不受iOS9新特性约束。 只有在xcode7 + iOS9才会产生ATS特性。

## 后台定位问题

iOS9的后台定位，如果不适配iOS9，则不能偷偷的在后台定位，后台定位的时候，会有蓝条显示。

请求后台定位权限

```Objective-C
// 1. 实例化定位管理器
_locationManager = [[CLLocationManager alloc] init];
// 2. 设置代理
_locationManager.delegate = self;
// 3. 定位精度
[_locationManager setDesiredAccuracy:kCLLocationAccuracyBest];
// 4.请求用户权限：分为：⓵只在前台开启定位⓶在后台也可定位，
//注意：建议只请求⓵和⓶中的一个，如果两个权限都需要，只请求⓶即可，
//⓵⓶这样的顺序，将导致bug：第一次启动程序后，系统将只请求⓵的权限，⓶的权限系统不会请求，只会在下一次启动应用时请求⓶
if ([[[UIDevice currentDevice] systemVersion] floatValue] >= 8) {
	//[_locationManager requestWhenInUseAuthorization];//⓵只在前台开启定位
	[_locationManager requestAlwaysAuthorization];//⓶在后台也可定位
}
// 5.iOS9新特性：将允许出现这种场景：同一app中多个location manager：一些只能在前台定位，另一些可在后台定位（并可随时禁止其后台定位）。
if ([[[UIDevice currentDevice] systemVersion] floatValue] >= 9) {
	_locationManager.allowsBackgroundLocationUpdates = YES;
}
// 6. 更新用户位置
[_locationManager startUpdatingLocation];
```

还需要添加info.plist配置

```
<key>NSLocationAlwaysUsageDescription</key>
<string>应用请求后台定位权限</string>
	
<key>UIBackgroundModes</key>
<array>
    <string>location</string>
</array>
```

## 企业级应用分发问题

iOS9取消了企业级证书信任按钮。
需要去设置-通用-设备管理 里面手动信任打包应用的证书。

## Bitcode

watch 必须包含bitcode iOS不强制支持bitcode  mac os 不支持

通俗解释 在线版安卓ART模式， ART模式 Android runtime 是安卓的系统优化的一种模式，在应用安装的时候，或进行一次预编译，将代码转化为机器语言存储在本地。在每次运行时就不需要进行一次编译了，提升了执行效率。同时也会有预编译后的容量问题，以及应用是否兼容该模式等问题。


