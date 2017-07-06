
## iOS编译优化策略Optimization Level

开发汉信码扫描功能时，扫描个别汉信码，`Release`环境下会闪退，`Debug`环境下正常。没有找到崩溃原因，暂时更改`Optimization Level`下的`Release`为`None[-O0]`，这样`Release`下也会和`Debug`一样，对代码没有优化操作。

更改位置：

```
TARGETS -> Build Settings -> Optimization Level -> Release  选择`None[-O0]`
```

[Optimization Level官方解释](https://gcc.gnu.org/onlinedocs/gnat_ugn/Optimization-Levels.html)

## 跳转京东客户端某商品详情页

iOS9以后，app内打开其他app需要在`Info.plist`里添加`LSApplicationQueriesSchemes`白名单。

跳转京东客户端需要增加京东的URL Scheme：`openapp.jdmobile`

[iOS URL Scheme 大全(快速启动)](http://www.jianshu.com/p/195b17052925)

首先要判断手机里是否安装京东客户端：

```Objective-C
if ([[UIApplication sharedApplication] canOpenURL:[NSURL URLWithString:@"openApp.jdMobile://"]]) {
		//安装了
}
```

跳转京东客户端：

```Objective-C
[[UIApplication sharedApplication] openURL:[NSURL URLWithString:@"openApp.jdMobile://"]];
```

跳转京东客户端某商品详情页：

```Objective-C
NSString *openUrl = @"openapp.jdmobile://virtual?params=%7B%22sourceValue%22:%220_productDetail_97%22,%22des%22:%22productDetail%22,%22skuId%22:%2299999999%22,%22category%22:%22jump%22,%22sourceType%22:%22PCUBE_CHANNEL%22%7D";
[[UIApplication sharedApplication] openURL:[NSURL URLWithString:openUrl]];
```

注意：openUrl字符串要原封不动的拷贝到应用中使用，里面的%22、%7等保持原样，不要转义。中间部分的`99999999`是京东的商品编号：`defSkuID`，跳转到该商品的详情中。

## OC与JavaScript交互

在APP内使用UIWebView展示网页，有时候需要和网页进行功能交互和数据传递。

最简单的一种交互是使用`UIWebView`的Delegate方法拦截webView的加载地址，和给定的地址匹配，如果匹配则不允许加载，并跳转到指定控制器，如果不匹配则允许加载。

拦截加载地址：

```Objective-C
- (BOOL)webView:(UIWebView*)webView shouldStartLoadWithRequest:(NSURLRequest*)request navigationType:(UIWebViewNavigationType)navigationType {
    NSURL *url = [request URL];
    NSLog(@"加载地址 = %@", url);
    return YES;//return YES允许加载，return NO不允许加载
}
```

iOS7以后，苹果推出了`JavaScriptCore`，用它可以直接完成oc和js之间的交互通信。下面介绍利用代理实现js调用oc中的方法，并完成数据传递。

第一步：在加载UIWebView的控制器中创建`JSExport`的协议，并写入和js约定好的方法，方法名要一致。该控制器遵守协议，并实现协议方法，完成webView和该控制器的交互。

```Objective-C
#import <UIKit/UIKit.h>
#import <JavaScriptCore/JavaScriptCore.h>//导入JavaScriptCore

@protocol JSObjcDelegate <JSExport> //创建协议，写入协议方法。方法名必须和js内中一致，这些方法在js中属于对象SafetyObj下的
//方法1：js调用oc中的方法
- (void)fnJumpScan;
//方法2：js调用oc中的方法，并要求得到某个返回值
- (NSString *)fnGetCustomData;
//方法3：js调用oc中的方法，并传递过来一个参数
- (void)fnSaveCustomData:(NSString *)callString;
@end

@interface ViewController : UIViewController<UIWebViewDelegate,JSObjcDelegate>

@property(nonatomic,weak)UIWebView *webView;
@property (nonatomic, strong) JSContext *context;//js运行环境
@end
```

第二步：在网页加载完毕时，获取js运行环境，并且设置方法对象的代理。

```Objective-C
- (void)webViewDidFinishLoad:(UIWebView *)webView {
    self.context = [webView valueForKeyPath:@"documentView.webView.mainFrame.javaScriptContext"];
    self.context[@"SafetyObj"] = self;//将自身设置为js对象的代理
    self.context.exceptionHandler = ^(JSContext *context, JSValue *exception) {
        context.exception = exception;
        NSLog(@"异常信息 = %@", exception);
    };
}
```

第三步：实现代理方法

```Objective-C
- (void)fnJumpScan {
    NSLog(@"点击了jumpScan");
}
- (NSString *)fnGetPostion {
    return @"1234";
}
- (void)fnSaveCustomData:(NSString *)callString {
    NSLog(@"callString = %@", callString);
}
```



