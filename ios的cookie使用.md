# iOS的cookie使用
### 废话
cookie是网络会话过程中记录客户端用户身份的一段文本信息。

http协议是一种无状态协议，每次数据交换都是一次单独的会话链接。一次数据交换结束，链接断开，再次交换数据，就需要重新链接。服务器无法从链接上跟踪客户端信息。cookie弥补了http协议的不足。

在网站访问中，cookie用的非常频繁。

客户端开发中，用到cookie的机会比较少。但是cookie是一直存在的，只是我们不用而已。

### 屁话
http请求，cookie会一直存在，任何一次http请求都会携带cookie到服务端。有时候我们可以把cookie作为识别用户身份的参数。而不必将uid放到请求参数中传递给服务器。


***

### 上代码


**保存cookie**

	NSArray *cookies = [[NSHTTPCookieStorage sharedHTTPCookieStorage] cookiesForURL:[NSURL URLWithString:@"接口地址"]];
	NSData *data = [NSKeyedArchiver archivedDataWithRootObject:cookies];
	[[NSUserDefaults standardUserDefaults] setObject:data forKey:@"UserDefaultsCookie"];

在获取用户信息的接口请求时获取cookie，并用userDefaults保存到plist里。

**设置cookie**
    
    NSData *cookiesdata = [[NSUserDefaults standardUserDefaults] objectForKey:kUserDefaultsCookie];
    if([cookiesdata length]) {
        NSArray *cookies = [NSKeyedUnarchiver unarchiveObjectWithData:cookiesdata];
        NSHTTPCookie *cookie;
        for (cookie in cookies) {
            [[NSHTTPCookieStorage sharedHTTPCookieStorage] setCookie:cookie];
        }
    }

*设置cookie在程序启动的时候设置*

程序启动时设置cookie，此次程序运行所有的http请求都会携带这个cookie，服务器会拿到这个cookie，在session中比对，找到对应的用户，做相应操作。
