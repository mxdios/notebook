在工作中分发内测应用是经常遇见的，常见的方式有：TestFlight，用企业级开发者账号，搜集UDID打包等方式，每种方式都各有利弊。

由于公司有企业级开发者账号，所以平常应用内测比较简单：直接拿企业级账号打包.ipa，将.ipa发布到内测平台（比如[蒲公英](https://www.pgyer.com)，[fir.im](https://fir.im/)），测试人员直接下载安装就可，只是第一次需要信任一下企业级证书。

有一次工作中遇到了这种情况：我们提供sdk，合作公司嵌入我们的sdk，我们需要测试结果。他们没有企业级账号，他们当然也不可能给我源码让我用企业级账号打包，我当然也不可能给他们企业级账号。纠结再三差点就用TestFlight或搜集UDID了。ipa重签名拯救了我。我让他们给我打一个.ipa包，我使用企业级账号重签名，分发内测平台供下载。

其实ipa重签名的应用场景很多，比如付费下载App Store上的应用，可以使用自己的开发者账号，将从其他平台下载下来的应用ipa重新签名，就可以安装了。各大app助手平台就是这么做的。注意直接从App Store下载的应用是加壳的，无法直接重新签名，必须砸壳。

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



资料：
[iOS的ipa重签名](http://www.jianshu.com/p/3f57d51f770a)

[代码签名探析](https://objccn.io/issue-17-2/)

[ios9适配系列教程](https://github.com/ChenYilong/iOS9AdaptationTips#3%E4%BC%81%E4%B8%9A%E7%BA%A7%E5%88%86%E5%8F%91)

[mac用终端对ipa包重新签名](http://www.hudongdong.com/skill/363.html)

[iPhone 如何不越狱安装越狱软件](http://www.jianshu.com/p/0d460b52ce96)

[实践篇：iOS 应用重签名(上)](http://www.iosugar.com/2017/04/10/Practice-articles-iOS-application-re-signature/)

[企业证书重新签名ipa](http://www.jianshu.com/p/1ef0dbdac653)


