# iOS开发笔记

**记录了在iOS开发中踩过的坑和一些问题解决**

## 目录

[微信的openid](https://github.com/mxdios/notebook/blob/master/notebooks/iOS%E5%BC%80%E5%8F%91%E7%AC%94%E8%AE%B0.md#%E5%BE%AE%E4%BF%A1%E7%9A%84openid)  
[Cell进入编辑状态，内容覆盖编辑按钮](https://github.com/mxdios/notebook/blob/master/notebooks/iOS%E5%BC%80%E5%8F%91%E7%AC%94%E8%AE%B0.md#cell%E8%BF%9B%E5%85%A5%E7%BC%96%E8%BE%91%E7%8A%B6%E6%80%81%E5%86%85%E5%AE%B9%E8%A6%86%E7%9B%96%E7%BC%96%E8%BE%91%E6%8C%89%E9%92%AE)  
[View中部分内嵌UIWebView](https://github.com/mxdios/notebook/blob/master/notebooks/iOS%E5%BC%80%E5%8F%91%E7%AC%94%E8%AE%B0.md#view%E4%B8%AD%E9%83%A8%E5%88%86%E5%86%85%E5%B5%8Cuiwebview)  
[设置UITextField的placeholder字体的颜色和字号](https://github.com/mxdios/notebook/blob/master/notebooks/iOS%E5%BC%80%E5%8F%91%E7%AC%94%E8%AE%B0.md#%E8%AE%BE%E7%BD%AEuitextfield%E7%9A%84placeholder%E5%AD%97%E4%BD%93%E7%9A%84%E9%A2%9C%E8%89%B2%E5%92%8C%E5%AD%97%E5%8F%B7)  
[按钮的拖动效果和点击事件并存](https://github.com/mxdios/notebook/blob/master/notebooks/iOS%E5%BC%80%E5%8F%91%E7%AC%94%E8%AE%B0.md#%E6%8C%89%E9%92%AE%E7%9A%84%E6%8B%96%E5%8A%A8%E6%95%88%E6%9E%9C%E5%92%8C%E7%82%B9%E5%87%BB%E4%BA%8B%E4%BB%B6%E5%B9%B6%E5%AD%98)  
[UITableViewCell左滑编辑字体颜色设置](https://github.com/mxdios/notebook/blob/master/notebooks/iOS%E5%BC%80%E5%8F%91%E7%AC%94%E8%AE%B0.md#uitableviewcell%E5%B7%A6%E6%BB%91%E7%BC%96%E8%BE%91%E5%AD%97%E4%BD%93%E9%A2%9C%E8%89%B2%E8%AE%BE%E7%BD%AE)  
[用Application Loader交付应用程序时，一直正在通过AppStore进行鉴定](https://github.com/mxdios/notebook/blob/master/notebooks/iOS%E5%BC%80%E5%8F%91%E7%AC%94%E8%AE%B0.md#%E7%94%A8application-loader%E4%BA%A4%E4%BB%98%E5%BA%94%E7%94%A8%E7%A8%8B%E5%BA%8F%E6%97%B6%E4%B8%80%E7%9B%B4%E6%AD%A3%E5%9C%A8%E9%80%9A%E8%BF%87appstore%E8%BF%9B%E8%A1%8C%E9%89%B4%E5%AE%9A)  
[pdf的展示](https://github.com/mxdios/notebook/blob/master/notebooks/iOS%E5%BC%80%E5%8F%91%E7%AC%94%E8%AE%B0.md#pdf%E7%9A%84%E5%B1%95%E7%A4%BA)  
[处理git库不允许上传大于100m的文件](https://github.com/mxdios/notebook/blob/master/notebooks/iOS%E5%BC%80%E5%8F%91%E7%AC%94%E8%AE%B0.md#%E5%A4%84%E7%90%86git%E5%BA%93%E4%B8%8D%E5%85%81%E8%AE%B8%E4%B8%8A%E4%BC%A0%E5%A4%A7%E4%BA%8E100m%E7%9A%84%E6%96%87%E4%BB%B6)  
[删除苹果开发者平台Developer里的App ID](https://github.com/mxdios/notebook/blob/master/notebooks/iOS%E5%BC%80%E5%8F%91%E7%AC%94%E8%AE%B0.md#%E5%88%A0%E9%99%A4%E8%8B%B9%E6%9E%9C%E5%BC%80%E5%8F%91%E8%80%85%E5%B9%B3%E5%8F%B0developer%E9%87%8C%E7%9A%84app-id)  
[NSUserDefaults存储数组或字典报错](https://github.com/mxdios/notebook/blob/master/notebooks/iOS%E5%BC%80%E5%8F%91%E7%AC%94%E8%AE%B0.md#nsuserdefaults%E5%AD%98%E5%82%A8%E6%95%B0%E7%BB%84%E6%88%96%E5%AD%97%E5%85%B8%E6%8A%A5%E9%94%99)  


# 微信的openid

微信的openid对于某个用户来说并不是唯一的，某个用户对某个公众号或者对某个微信开放平台下某个appid生成的是唯一的。appid改变，openid也会改变。公众号下和app下获得的openid不同，现阶段无法实现相同操作。使用unionid打通用户，微信开放平台上绑定了公众号，这时获取的unionid会一致。


# Cell进入编辑状态，内容覆盖编辑按钮

删除一条cell时，最常用的方法就是侧滑删除。

还有就是点击编辑按钮，cell进入编辑状态，点击红色减号button，出现删除按钮，点击删除完成删除操作。这时会出现问题，cell上的内容遮盖了删除按钮，如下图所示。

![如图](http://oalg33nuc.bkt.clouddn.com/image/Untitleds.gif)

解决这个问题，只需要在自定义cell里加如下代码

```Objective-C
- (void)layoutSubviews
{
    [super layoutSubviews];
    if (self.isEditing) {
        [self sendSubviewToBack:self.contentView];
    }
}
```

![结果](http://oalg33nuc.bkt.clouddn.com/QQ20160722-0.png)


# View中部分内嵌UIWebView

在开发中，有的界面要求部分原生，部分根据服务器返回的URL地址显示网页。这就需要计算这部分网页的size。不然就会显示不全或者留白太多

创建UIWebView 并加载页面

```Objective-C
UIWebView *webview = [[UIWebView alloc] init]; //创建webview
webview.delegate = self;  //设置代理
[webview loadHTMLString:url baseURL:nil]; //加载网页
[self.view addSubview:webview]; //添加到view上
```

在webView的代理方法中获取网页实际size

```Objective-C
- (void)webViewDidFinishLoad:(UIWebView *)webView
{
    webView.width = self.view.width; //实际宽度
    webView.height = [webView stringByEvaluatingJavaScriptFromString:@"document.body.scrollHeight"].floatValue; //实际高度
}
```

# 设置UITextField的placeholder字体的颜色和字号

```Objective-C
textField.placeholder = @"请输入用户名";  
[textField setValue:[UIColor redColor] forKeyPath:@"_placeholderLabel.textColor"];  
[textField setValue:[UIFont boldSystemFontOfSize:16] forKeyPath:@"_placeholderLabel.font"];
```

# 按钮的拖动效果和点击事件并存

需求是一个按钮可以在屏幕中随意拖动，不能拖出屏幕外。距离屏幕最边缘有10像素的距离。拖动结束后，根据距离左右两侧哪侧近，靠到近的一侧。

定义`Bool`类型的全部变量，控制是否执行点击事件

```Objective-C
BOOL _isClick;
```

创建按钮添加拖动和点击事件

```Objective-C
//添加点击事件
[btn addTarget:self action:@selector(btnClick:) forControlEvents:UIControlEventTouchUpInside];
//添加拖动事件
[btn addTarget:self action:@selector(dragMoving:withEvent:)forControlEvents: UIControlEventTouchDragInside];
//添加拖动结束时的事件
[btn addTarget:self action:@selector(dragEnded:withEvent:)forControlEvents: UIControlEventTouchUpInside];
```

实现事件方法

```Objective-C
//拖动过程中
- (void)dragMoving:(UIControl *)c withEvent:ev
{
    CGPoint point = [[[ev allTouches] anyObject] locationInView:self.view];   
    point.x = MIN(MAX(point.x, btn.width * 0.5 + 10) , self.view.width - btn.width * 0.5 - 10);//范围
    point.y = MIN(MAX(point.y, 100), self.view.height - btn.height * 0.5 - 10);//范围
    c.center = point;
    _isClick = NO;
}
//拖动结束
- (void)dragEnded:(UIControl *)c withEvent:ev
{
    XDLog(@"dragEnded....");   
    CGPoint point = [[[ev allTouches] anyObject] locationInView:self.view];
    point.x = MIN(MAX(point.x, btn.width * 0.5 + 10), self.view.width - btn.width * 0.5 - 10);//范围
    point.y = MIN(MAX(point.y, 100) , self.view.height - btn.height * 0.5 - 10);//范围
    c.center = point;
    [UIView animateWithDuration:0.2 animations:^{
        c.centerX = c.centerX < self.view.width - c.centerX ? 30 : self.view.width - 30;
    }];
    _isClick = YES;
}
//点击事件
- (void)btnClick:(UIButton *)btn
{
    if (_isClick) {
    	//点击方法
    }
}
```

# UITableViewCell左滑编辑字体颜色设置

设置按钮，iOS8之后API，用下面代码设置多个按钮

```Objective-C
- (NSArray<UITableViewRowAction *> *)tableView:(UITableView *)tableView editActionsForRowAtIndexPath:(NSIndexPath *)indexPath
{
	void(^rowActionHandler)(UITableViewRowAction *, NSIndexPath *) = ^(UITableViewRowAction * _Nonnull action, NSIndexPath * _Nonnull indexPath) {
        NSLog(@"%@  index = %ld", action.title, indexPath.row);
        [tableView setEditing:NO animated:YES];
    };
    UITableViewRowAction *action1 = [UITableViewRowAction rowActionWithStyle:UITableViewRowActionStyleNormal title:@"编辑" handler:rowActionHandler];
    UITableViewRowAction *action2 = [UITableViewRowAction rowActionWithStyle:UITableViewRowActionStyleNormal title:@"删除" handler:rowActionHandler];
    UITableViewRowAction *action3 = [UITableViewRowAction rowActionWithStyle:UITableViewRowActionStyleNormal title:@"喜欢" handler:rowActionHandler];
    action1.backgroundColor = [UIColor lightGrayColor];
    action2.backgroundColor = [UIColor blackColor];
    action3.backgroundColor = [UIColor redColor];
    return @[action3,action1,action2];
}
```
这种方式显示的样式是系统自带的，系统指定字体和文字颜色。

按钮可以修改文字颜色，添加标题图片，背景图片等。`UITableViewRowAction`的显示为`UIButton`，修改`UIButton`的显示样式，cell的多个编辑按钮都会变化

```Objective-C
[[UIButton appearance] setTitleColor:[UIColor blackColor] forState:UIControlStateNormal];
[[UIButton appearance] setImage:[UIImage imageNamed:@"searchtool"] forState:UIControlStateNormal];
[[UIButton appearance] setBackgroundImage:[UIImage imageNamed:@"searchtool"] forState:UIControlStateNormal];
```

值得注意的是下面这种方式设置字号无效

```Objective-C
[UIButton appearance].titleLabel.font = XDFont(10);
```

只有一个按钮的话可以使用`NSAttributedString`设置显示文字的字体、字号、颜色等

```Objective-C
NSDictionary *attributes = @{NSFontAttributeName:[UIFont systemFontOfSize:10], NSForegroundColorAttributeName:[UIColor redColor]};
NSAttributedString *attributedTitle = [[NSAttributedString alloc] initWithString:@"编辑" attributes: attributes];
[[UIButton appearance] setAttributedTitle:attributedTitle forState:UIControlStateNormal];
```

*这种实现方式并不好有诸多问题，当有多个按钮分别设置不同的字体字号颜色时，这种方式无法实现，当找到有效方法再添加....此条权当问题记录*

# 用Application Loader交付应用程序时，一直正在通过AppStore进行鉴定

需要修改Application Loader里的一个文件内容。

路径为

```
/Applications/Xcode.app显示包内容 -> Contents/Applications/Application Loader.app显示包内容 -> Contents/itms/java/lib/net.properties  
```
或者路径为

```
/Applications/Xcode.app显示包内容 -> Contents/Applications/Application Loader.app显示包内容 -> Contents/MacOS/itms/java/lib/net.properties 
```
打开这个文件，把第34行的http.proxyPort=443 改为https.proxyPort=80  再提交应用就ok了

# pdf的展示

项目里要求读取从服务器下载下来的pdf，pdf文件是一个发票文件，下载到本地Documents目录下，展示出来。问题来了，发票上的印章不见了。

我将这个pdf文件放到了各种应用中打开，结果是：微信、qq、印象笔记、掌阅iReader、Safari、iBooks、WPS Office、mac上的预览，除了WPS Office、mac上的预览能显示出印章，其他的全部显示不出印章，甚至微信、掌阅iReader连发票上的分割线都显示不出来。

而且，用WPS Office打开，可以点击印章删除，可以点击分割线删除。这时我感觉印章和分割线是在pdf上的标注，就像以前使用Adobe的工具给pdf加标注文字，选中文字高亮显示。

**最开始使用了最简单的展示pdf文件的方法：UIWebView**

```Objective-C
NSArray *paths = NSSearchPathForDirectoriesInDomains(NSDocumentDirectory, NSUserDomainMask, YES);
NSString *documentsDirectory = [paths objectAtIndex:0];
NSString *filePDF = [documentsDirectory  stringByAppendingPathComponent:@"file.pdf"]

//第一种webview加载方式
NSURL *url = [NSURL fileURLWithPath:_filePath];
NSURLRequest *request = [NSURLRequest requestWithURL:url];
[_webView loadRequest:request];

//第二种webview加载方式
NSURL *url = [NSURL fileURLWithPath:_filePath];
NSData *data = [NSData dataWithContentsOfFile:_filePath];
[_webView loadData:data MIMEType:@"application/pdf" textEncodingName:@"UTF-8" baseURL:url];
```

失败！

**转而使用`Quartz 2D`绘制pdf**


```Objective-C
- (void)drawRect:(CGRect)rect
{
	CGContextRef context = UIGraphicsGetCurrentContext();
    //旋转坐标系
    CGContextTranslateCTM(context, 0, self.frame.size.height-60);
    CGContextScaleCTM(context, 1, -1);
    CGPDFPageRef pdfPage = CGPDFDocumentGetPage(_pdfDoc, 1);
    CGContextSaveGState(context);
    CGAffineTransform pdfTransform = CGPDFPageGetDrawingTransform(pdfPage, kCGPDFCropBox, self.bounds, 0, true);
    CGContextConcatCTM(context, pdfTransform);
    CGContextDrawPDFPage(context, pdfPage);
    CGContextRestoreGState(context);
}
```
	
失败！甚至分割线都没了

这是最常用的两种读取pdf文件的方式，然而还是显示不出来发票上面的印章

**使用QLPreviewController预览**

```Objective-C
//导入QuickLook库
#import <QuickLook/QuickLook.h>
- (void)viewDidLoad {
	QLPreviewController *previ = [[QLPreviewController alloc] init];
	previ.view.frame = CGRectMake(0, 0, self.view.width, self.view.height - 200);
	previ.delegate = self;
	previ.dataSource = self;
	[self.view addSubview:previ.view];
}
//实现代理方法
#pragma mark - 在此代理处加载需要显示的文件
- (NSURL *)previewController:(QLPreviewController *)previewController previewItemAtIndex:(NSInteger)idx
{
    return [NSURL fileURLWithPath:filePath];
}
#pragma mark - 返回文件的个数
-(NSInteger)numberOfPreviewItemsInPreviewController:(QLPreviewController *)controller {
    return 1;
}
#pragma mark - 即将要退出浏览文件时执行此方法
-(void)previewControllerWillDismiss:(QLPreviewController *)controller {
    XDLog(@"退出");
}
```

还是失败的没显示出来印章。

**在github上找读取pdf的项目**

- UIImage-PDF 将pdf转成image显示，结果失败
- Reader 比较著名的pdf阅读项目，支持大文件，加密文件，有着ibooks一样的界面。也有着iBooks一样的结果，同样显示不出来印章
- PDFRenderer 结果失败
- PDFTest 结果失败
- PDFViewTest 结果失败
....

然后各种谷歌、百度、必应搜索，终于搜到了一个有用的结果

原文如下 

*公司的项目要读取服务器发来的PDF文件，用网上的方法读取后虽然能读出来，但是发现个问题，PDF文件有的签章不见了。这个可是大问题，试了好多iOS框架库，例如 Reader,FastPdfKit都读不出来。*

*在知乎上找到一个大神的发言，解释了PDF有的内容读不出来的原因。iOS 对矢量图片的支持如何？ 直接引用其中的一段话:"iOS 的 Core Graphics 框架底层和 OS X 一样，都是基于 PDF 的。所以 iOS 用 PDF 很方便，比如 iOS 的 Quick Look 框架就可以直接看 PDF。如果要是只看文档那样简单的 PDF 的话，一般是没有什么问题的。不过，如果要是想看用 AI 制作的，带有多重描边、填充、网格渐变、阴影、多图层等东西的复杂 PDF 图形的话，有很大机率会出现问题。比如，在 iOS 和 OS X 上常出现的一个问题是，PDF 文稿里隐藏的图层、图形被显示出来。如果查看复杂一些的 PDF，很可能在 Adobe Illustrator、OS X、iOS、Adobe Reader 下查看的效果都不一样。所以说，PDF 虽然一般被认为是跨平台的「安全格式」，不过也并不是 100% 保险。 Adobe Reader 的 iOS 版很可能没有使用 iOS 系统自带的 PDF 和矢量绘图 API，而是自己实现了一个，因此通常 Adobe Reader 显示复杂一些的 PDF 比使用 iOS 原生 API 解析 PDF 的 App 更准确一些。还有， 无论是使用 iOS 原生的 Quick Look 还是用 Adobe Reader 查看，渲染复杂的 PDF 有可能特别慢"。公司PDF的签章好像就是多图层，在文字的底下。*

**找了好久，终于找到了一个能读取多图层的PDF文件的框架 - MuPDF**

得出一个结论，使用Mupdf

[Mupdf github地址](https://github.com/muennich/mupdf)

[使用方法](http://www.jianshu.com/p/5fd00530d4bb)


# 处理git库不允许上传大于100M的文件

git库在上传大于50M的文件时会有警告，文件大于100M时直接拒绝push。要将这个文件从本地库和远程库中移除掉，再进行push操作。比如百度地图的导航静态库`libbaiduNaviSDK.a`就有100多M，做push操作时，直接报错了。

> remote: warning: Large files detected.  
> remote: error: File gasstation/gasstation/Classes/Main/Lib/BaiduNaviSDK/libbaiduNaviSDK.a is 108.63 MB; this exceeds Git@OSC's file size limit of 100 MB  
> remote: error: hook declined to update refs/heads/master

如果这个文件是最近一次commit的，并没有进行push操作，换句话说进行push操作的时候报错了。那么需要删除掉本地库中这个文件的commit记录再进行push

在终端中cd进项目目录，就是项目的`.git`文件的目录下

```
cd /Users/XXX(cd后面路径换成自己项目的路径)
```

输入如下命令

```
git rm --cached /Users/XXX/XXX/libbaiduNaviSDK.a (大文件的路径)
git commit --amend -CHEAD
```

到这一步这个大文件从commit记录中移除了，以后的commit也不会再commit它了。此时进行push即可

还有一种情况是你无数次commit了，也不知道哪次把这个大文件commit上了，在进行push的时候报错了。

这时需要将本地代码库回滚，回滚到某个commit之前

```
git reset --hard commit-id :回滚到commit-id，讲commit-id之后提交的commit都去除

git reset --hard HEAD~3：将最近3次的提交回滚
```

# 删除苹果开发者平台Developer里的App ID

删除苹果开发者平台Developer里的某个AppID本来是很容易的事情，点击Edit -> Delete -> 警示框中红色Delete即可。但是真实情况却坑的一逼……

Developer中的AppID对应项目中的Bundle id，是一个app的唯一标识，包括在App Store中，在各大第三方开放平台中。一般为了保证唯一性，采用公司域名反写＋app项目名的方式命名。

我经历过替换开发者账号的事。事情是这样的：开发某款app在上线AppStore当天，甲方觉得提供商名字不好(因为是个人开发者账号，显示的是人名拼音)要求下架，重新提供开发者账号提交。甲方是上帝，您高兴就好，反正$99不用我掏腰包。就这样上架第一天我亲手点了下架按钮，

问题就来了，Bundle id被旧的开发者帐号占用了，新的没法用了，我不想为了换个开发者帐号把Bundle id改了，毕竟牵扯好几个第三方平台呢。那就删除原来账号的Bundle id，删除的时候有如下提醒：

![img](https://github.com/mxdios/notebook/blob/master/notebooks/images/QQ20160928-0.png?raw=true)

在App Store上使用了，不能删除。我只好去iTunes Connect里删除了app（**注意：此为大坑**），问题依旧，只能求助苹果开发者平台的客服人员。

打苹果开发者客服电话: 4006 701 855 [全球电话支持](https://developer.apple.com/contact/phone/cn/)。坑爹的是，苹果客服人员告诉我删除不了，开发者平台中的Bundle id在app构建版本之后，不能修改或删除的。即便是你用该Bundle id发布的应用下架删除了，该Bundle id也会随着该应用永沉大海。不能删！不能改！

客服告诉我最佳的解决办法是迁移应用。将原账号的应用迁移到新账号上，应用所用到的Bundle id也会随之迁移过去。但是我把iTunes Connect里的应用删除了，那就没办法迁移了。摆在我面前的有两条路，第一：重新用旧账号打包提交审核通过，迁移应用到新账号，第二：改Bundle id。苹果客服人员对我如是说，客服诚不欺我啊！

# NSUserDefaults存储数组或字典报错

用NSUserDefaults存储数组或字典的时候报错，程序崩溃了，报错内容如下：

> Terminating app due to uncaught exception 'NSInvalidArgumentException', reason: 'Attempt to insert non-property list object {xxxxxxxxxx} for key defaultsKey'

是因为要存的数组或字典对象里存在类似`"name" = "<null>"`这样的字段，这样的字段不能被解析，所以会报错。

解决办法是将`"name" = "<null>"`换成`"name" = ""`，这种替换最好是服务端来做，如果服务端没做的话，移动端处理也可以，处理方式如下：

```Objective-C
NSMutableDictionary *dict = [NSMutableDictionary dictionaryWithDictionary:oldDict];
for (NSString *keyStr in dict.allKeys) {
   if ([[dict objectForKey:keyStr] isEqual:[NSNull null]]) {
       [dict setObject:@"" forKey:keyStr];
   } else {
       [dict setObject:[dict objectForKey:keyStr] forKey:keyStr];
   }
}
NSLog("newDict = %@", dict);
```

# UITableViewCell刷新闪烁问题

```swift
UIView.performWithoutAnimation {
	tableView?.reloadRows(at: [IndexPath(row: 0, section: 1)], with: .none)
}
```

