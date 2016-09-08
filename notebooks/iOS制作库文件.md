# iOS库

iOS库一般有开源库，静态库和动态库。

开源库是指源代码开放的功能代码，比如AFNetworking。

静态库是指内部代码逻辑看不到，依靠相应的.h头文件调用内部方法的功能模块。有`.a静态库`和`.framework`两种形式。比如：百度导航的`libbaiduNaviSDK.a`，百度地图`BaiduMapAPI_Map.framework`。

动态库同样看不到内部代码逻辑，有`.dylib`(现在为`.tbd`)和`.framework`两种形式。比如常见的`libz.tbd`和`Foundation.framework`

**注意：**

**苹果禁止使用自己创建的动态库。**

**swift无法制作.a静态库，只能创建.framework静态库**

# 制作.a静态库

.a静态库是iOS开发中常用的一种库文件，通常为一些第三方提供的功能模块。

以我写的一个工具方法[给字符串设置富文本模式](https://github.com/mxdios/XDAttributedTool)为例创建.a静态库

## 1.创建工程

创建项目，选择iOS -> Framework & Library -> Cocoa Touch Static Library

![创建工程](https://github.com/mxdios/notebook/blob/master/notebooks/images/QQ20160908-0.png?raw=true)

将要打包.a静态库的功能代码拖到新建的项目中。

![创建工程](https://github.com/mxdios/notebook/blob/master/notebooks/images/QQ20160908-1.png?raw=true)








