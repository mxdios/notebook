# 提交应用到AppStore的准备工作

## App信息

![img](https://github.com/mxdios/notebook/blob/master/notebooks/images/QQ20160815-7.png?raw=true)

### 公司名称

![img](https://github.com/mxdios/notebook/blob/master/notebooks/images/WechatIMG1081.jpeg?raw=true)

公司名字是指显示在这里的名字。这个只针对公司级账号说的，个人账号不能设置公司名称，只能以开发商名字显示。

![img](https://github.com/mxdios/notebook/blob/master/notebooks/images/QQ20160930-0.png?raw=true)

每个账号在iTunes Connect里创建第一个应用的时候，可以设置公司名字，如果不设置的话就显示开发商名字。日后不能修改公司名字，也不能重新设置。只能给苹果开发者发邮件，要求去掉公司名显示开发商名字。

### app名称

您的 App 在 App Store 中显示的名称。名称长度不能超过~~255个字符~~ 50个字符（新审核指南修改）。并不等同于app安装完成在手机上显示的名字，这里的名称可以写为名字+简介。

例如：天猫在AppStore展示的名称如下图，应用名为“天猫”![天猫](http://oalg33nuc.bkt.clouddn.com/image/%E5%9B%BE%E7%89%87%201.png)

### 隐私政策网址(可不填)

您所在机构的隐私政策网址(URL)。面向儿童或提供自动续费的 App 内购买项目或免费订阅的 App, 均需要隐私政策。另外, 需用帐户注册、使用现有帐户访问、或由法律另行规定的 App 也需隐私政策。对于收集用户或设备相关数据的 App, 亦推荐使用隐私政策。

### 套装ID 

套装 ID 必须与您在 Xcode 中使用的 ID 相符。该 ID 在您上传第一个构建版本后便无法更改。

开发人员设置的，在开发信息配置里`Certificates, Identifiers & Profiles`注册的`Identifiers`。一般与工程中的`Bundle Identifier`相同，如果`Identifiers`使用了通配符`*`则不需要完全一样(应用有消息推送功能则不能使用通配符)。

### SKU

您 App 专有的 ID, 此 ID 不会在 App Store 中显示。

开发人员设置的，随便写一个即可保持唯一性。

### Apple ID 

为您的 App 自动生成的 ID。无需设置，在`iTunes Connect`创建了应用就会生成此ID。做qq登录分享功能在腾讯开放平台注册应用时需要提交这个ID。

### 类别

最能准确描述此 App 的类别。分为主要类别和次要类别，次要类别可不填。有如下类别：

报刊杂志、财务、参考、导航、儿童、工具、购物、健康健美、教育、旅游、美食佳饮、商品指南、商务、社交、摄影与录像、生活、体育、天气、图书、效率、新闻、医疗、音乐、游戏、娱乐。

详情请见：[AppStore类别定义](https://itunesconnect.apple.com/itc/static/category_definitions)

### 分级

此 App 分级将在所有平台的 App Store 中显示。此分级根据分级限制最高的 App 平台而定。

## App版本信息

### app预览和屏幕快照

屏幕快照必须为 JPG 或 PNG 格式，且必须采用 RGB 颜色空间。 App 预览必须为 M4V、MP4 或 MOV 格式，且不能超过 500 MB。

现在只需要提交5.5英寸的图片，其他尺寸会沿用5.5的图片，尺寸为1242*2208。可以设计几张app介绍图或者用手机截屏。也可以用模拟器保存界面快照，操作：`command + s`，注意模拟器有显示百分比，在`模拟器->Window->Scale`里面，快捷键是`command + 12345`，保存界面快照时要`command + 1`100%模拟器显示。非Retina屏(低分辨率)的Mac模拟器会超过屏幕被压扁了，不影响快照保存。

![img](https://github.com/mxdios/notebook/blob/master/notebooks/images/QQ20160815-4.png?raw=true)

### 描述/关键字/技术支持网址/营销网址

描述：对您 App 的描述，用以详细说明特性和功能。它还将用于您的 Apple Watch App。显示于AppStore中的内容提要

关键字：一个或多个关键词，用以描述您的 App。关键词将使 App Store 搜索结果更加准确。

技术支持网址：您的 App 技术支持信息网址(URL)。该网址(URL)将会在 App Store 中显示。显示位置为`开发人员网站`。点击在Safari中打开网址。一般会填写官网地址。

营销网址：您的 App 营销信息网址(URL)。该网址(URL)会在 App Store 中显示。可不填写。

![img](https://github.com/mxdios/notebook/blob/master/notebooks/images/QQ20160815-5.png?raw=true)

## 构建版本

提交app到AppStore审核最重要的地方！提交打包好的.api文件

![img](http://oalg33nuc.bkt.clouddn.com/image/QQ20160815-11.png)

但并不是在这里面上传.api文件。而是需要用`Application Loader`交付应用程序。

## App综合信息

### app图标

此图标将用于 App Store，其格式必须为 JPG 或 PNG，最低分辨率至少为 72 DPI，并采用 RGB 色彩空间。它不能包含图层或圆角。尺寸为1024*1024

### 版权

拥有您的 App 专有权的人员或实体的名称，前面是获得权利的年份（例如“2008 Acme Inc”）。请勿提供网址(URL)。显示在AppStore

### 商务代表联系信息 

您可以为您的 App 在韩国 App Store 中提供额外信息，这一额外信息只会显示在韩国 App Store 中。

![img](https://github.com/mxdios/notebook/blob/master/notebooks/images/QQ20160815-9.png?raw=true)

### 分级

为app指定分级，一般都会指定低级，但也要根据实际情况。会影响AppStore审核

[了解详情](https://itunesconnect.apple.com/itc/views/shared/app_store_rating_matrix.html)

![img](https://github.com/mxdios/notebook/blob/master/notebooks/images/QQ20160815-10.png?raw=true)

## APP审核信息

![img](https://github.com/mxdios/notebook/blob/master/notebooks/images/QQ20160815-3.png?raw=true)

### 联系信息：
一般会填写开发者的联系信息。在App审核中有问题会联系开发者。但app审核结果不会通过这个联系方式反馈，会反馈给开发者账号邮箱。可以下载`iTunes Connect`应用，登录提交app的开发者账号，随时接收审核状态消息推送。~~我不会说这个应用一点都不好用，但只此一家别无分号，爱用不用~~

### 演示账户：

提供给AppStore审核人员用于登录app的账号密码，必须有效可登录，**非常重要,别问我是怎么知道的/(ㄒoㄒ)/~~**。有的应用是手机号+验证码登录，需要事先让服务器同事留好后门，设定固定手机号+固定验证码能随时登录。(按说这应该开发时就得做到，不然开发者每次登录操作都获取验证码多麻烦，而且这还是要花钱的)

### 备注：

对审核过程会有所帮助的、有关您 App 的额外信息, 包括在测试中需要的 App 特别设置等。额外说明app里某些影响到审核的部分，可以放入图片链接或者操作视频链接以弥补审核人员不能操作到的部分(比如：p2p项目中绑定银行卡)。

### 版本发布：

这个根据实际情况设置，随便设置，为了一切尽在掌控，我会选择手动发布此版本，你应该体会一下应用审核通过点击发布按钮那一刻的感觉！（千万别点击开发者撤下提交的版本...）

## 之前不幸踩过的一些坑，被AppStore拒了

1. **微信登录/支付/分享：**微信没有提供网页登录。如果手机没有安装微信，不能提醒用户下载微信。正确做法是隐藏掉微信按钮。

2. **qq登录：**qq登录提供了网页登录，但是如果手机没安装微信，腾讯有时会一(chou)意(bu)孤(yao)行(lian)的跳转到qq应用下载页。如果是这样的话一定被拒！正确做法时一定要测试没有qq客户端的时候登录会不会到网页qq登录，如果跳到了qq下载页，联系腾讯客服人员，提供appid，他们会给改好。

3. **举报功能：**如果应用中存在用户发布的内容，比如用户可以发布状态、帖子等(类似微博)，就必须有举报功能。

4. **新版本检测：**这一条是后来加的审核规则(苹果的审核规则号称大道无形，就是一切我说了算，说你不行你就不行行也不行，见过最搞笑的被拒案例是：一个娱乐型app被拒了，理由是我们感觉您的app不具有娱乐性。全公司懵逼...)。检测是否有新版本之前是可以的，后来苹果不允许了，新版本检测都放到AppStore里进行。但是这种行为并不是不允许的，苹果不允许的是用户在app内手动检测新版本，换句话说app内不允许存在检测新版本的按钮操作，可以隐式的调用后台接口检测后台的版本库，弹窗提醒用户有新版本。

5. **iOS/iPhone/iPad/Mac等名词：**这些苹果专属的名词在app中如果有展示，一定不要写错了，注意大小写，如果错了一定被拒。不展示的话就无所谓了，比如说在代码中定义一个变量名写IpHonE都没人管你(写这样的变量名不排除同事会砍死你)。

6. **未完成模块：**AppStore不接受测试版或未完成版，某个模块如果未完成千万不要为了用户体验贱贱的写上模块开发中...之类的提示，这样会被拒绝上架的。留一个空白页面或者按钮点击没反应可能会上架成功。

7. **64位/https/bitcode：**应用必须支持64位包括应用中使用的静态库，如果不支持64位，交付应用程序就会失败，根本到不了审核。https安全网络请求，目前为止并没有强制这样做，不保证以后会不会强制。比如现在app必须包含`IPv6-only`网络支持。bitcode Watch应用必须支持，iOS应用可支持可不支持，Mac应用不支持。

8. **后台地理定位：**使用后台地理定位Background Modes为打开状态，需要在`info.plist`的`NSLocationAlwaysUsageDescription`字段添加文字描述，说清楚后台地理定位用在什么地方了。
>Please revise the NSLocationAlwaysUsageDescription value in the info.plist to specify the intended purpose of using the user's location while the app is in the background.

	还有一个很重要的地方，就是必须在提交app到AppStore时填写的描述里添加电池使用免责声明，提示用户后台定位操作会消耗电量并且影响电池续航能力。这个规则略坑啊
	
	>Please also add the following battery use disclaimer in your Application Description:
"Continued use of GPS running in the background can dramatically decrease battery life."






