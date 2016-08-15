# iOS开发笔记

**记录了在iOS开发中踩过的坑和一些问题解决**

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

