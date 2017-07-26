在工作中分发内测应用是经常遇见的，常见的方式有：TestFlight，用企业级开发者账号，搜集UDID打包等方式，每种方式都各有利弊。

由于公司有企业级开发者账号，所以平常应用内测比较简单：直接拿企业级账号打包.ipa，将.ipa发布到内测平台（比如[蒲公英](https://www.pgyer.com)，[fir.im](https://fir.im/)），测试人员直接下载安装就可，只是第一次需要信任一下企业级证书。

在工作中，有时候我们需要分发某个app的.ipa包，并没有改app的源码，无法用自己的账号打包。这时就用到了重签名。

其实ipa重签名的应用场景很多，比如App Store上的付费应用，可以使用自己的开发者账号，其.ipa包重新签名，就可以安装了。各大app助手平台就是这么做的。注意直接从App Store下载的应用是加壳的，无法直接重新签名，必须砸壳。[iOSRE逆向](http://iosre.com/)

## iReSign

[iReSign](https://github.com/maciekish/iReSign)是一个重签名工具

![iReSign](http://oalg33nuc.bkt.clouddn.com/2017-07-25-19-21-04.png)

操作步骤：

1. 选择.ipa包的路径
2. 选择`embedded.mobileprovision`文件路径。`embedded.mobileprovision`是用于打包的配置文件`Provisioning Profiles`。可以直接在`Certificates, Identifiers & Profiles`中下载，注意下载之后需要将名字设置为`embedded.mobileprovision`。也可以在之前用该文件打包过的.ipa中找：解压.ipa -> Payload -> xx.app显示包内容 -> `embedded.mobileprovision`
3. 网上有资料说entitlements.plist不需要填写。我试过不填写或填写，我按照本文[iOS证书及ipa包重签名探究](http://www.olinone.com/?p=198)的方式写的.plist文件。
4. 选中修改ID，填写修改的appid
5. 选择证书，下拉列表会显示mac上有的证书。
6. 点击重新签名！

结果是失败了。

![iResign运行结果](http://oalg33nuc.bkt.clouddn.com/6111501034232_.pic.jpg)

错误信息如下，可以查看该错误的[IResign上的issues](https://github.com/maciekish/iReSign/issues/99)

```
[General] Error Domain=NSCocoaErrorDomain Code=3840 "Unexpected character s at line 1" UserInfo={NSDebugDescription=Unexpected character s at line 1, kCFPropertyListOldStyleParsingError=Error Domain=NSCocoaErrorDomain Code=3840 "Unexpected ';' or '=' after key at line 1" UserInfo={NSDebugDescription=Unexpected ';' or '=' after key at line 1}}
```

另外还有一个重签名工具：[ios-app-signer](https://github.com/DanTheMan827/ios-app-signer)

[实践篇：iOS 应用重签名(上)](http://www.iosugar.com/2017/04/10/Practice-articles-iOS-application-re-signature/)这篇文章介绍了entitlement文件如何写，ios-app-signer的使用。我没有测试该方法是否可行。

## 使用sigh脚本

在iReSign失败后，我在网上找到使用sigh脚本的方式来重签名。

1. 安装神器brew，没有安装的看这里：[Homebrew](https://brew.sh/)
2. 安装ruby，命令：`brew install ruby`
3. 安装sigh，命令：`sudo gem install sigh`，过程比较漫长。

安装sigh脚本后，开始进行重签名，步骤如下

1. 在终端输入`sigh resign`，回车
2. 把要签名的ipa文件拖到窗口上，回车
3. 填写用来签名的证书名，回车
4. 把项目的配置文件embedded.mobileprovision文件拖到窗口上，回车
5. 好了，resign脚本会自动更改bundel id，签名并重新打包。

注意ipa包名不要有中文。如果应用有多个targetes无法用上述方式重签名。参考文章：[iOS的ipa重签名](http://www.jianshu.com/p/3f57d51f770a)

我按照如上步骤操作，成功使用企业级证书打包，上传到蒲公英内测平台上，也显示的是企业级应用，但是无法安装，提醒无法下载app，我手机上没有从App Store上下载安装该应用，排除了无法覆盖问题。

iOS9以后，企业级应用不会出现信任按钮，如果ipa包和网页的`bundle ID`不匹配，就无法安装。在iOS9之前是没有检测的。详情见大神iOS程序犭袁的文章[ios9适配系列教程](https://github.com/ChenYilong/iOS9AdaptationTips#3%E4%BC%81%E4%B8%9A%E7%BA%A7%E5%88%86%E5%8F%91)

## 我操作成功的办法

经过了无数次测试sigh脚本签名方式，最后即将放弃的时刻成功了。

1. 将需要重签的.ipa文件和`embedded.mobileprovision`配置文件放到同一个文件夹下。注意路径和文件名中不要出现中文，配置文件名一定要是`embedded`。
2. 在终端中cd到该路径下
3. 使用sigh脚本：`sigh resign`回车，运行结果跟上面是不一样的，会直接出现`Signing Identity:`让我填证书名。上述方式应该是第二步拖ipa文件到窗口，`Path to ipa file:`
4. 在`Signing Identity:`上面会出现一串可用的`identity`，这里要输入这些`identity`的十六进制串。回车
5. 成功了：`Successfully signed 路径/xxx.ipa!`原来文件夹中的.ipa已经被重签名了。上传到蒲公英内测平台，完全可以下载使用。

![操作步骤](http://oalg33nuc.bkt.clouddn.com/QQ20170726-104243.png)

本文涉及到的知识点都是从网上Google的，感谢网友们分享的资料，还涉及的一些参考资料：

[代码签名探析](https://objccn.io/issue-17-2/)

[mac用终端对ipa包重新签名](http://www.hudongdong.com/skill/363.html)

[iPhone 如何不越狱安装越狱软件](http://www.jianshu.com/p/0d460b52ce96)

[企业证书重新签名ipa](http://www.jianshu.com/p/1ef0dbdac653)


