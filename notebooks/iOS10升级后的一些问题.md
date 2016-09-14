# iOS10升级

在2016年9月14日凌晨iOS10系统准时推送过来了，作为懒人之前的beta版一直没有升级试用。今天果断手机和Xcode一起升级了，新系统新面貌带来新bug。记录一下升级iOS10和Xcode8.0后遇到的问题。

## 开发者证书不能使用

General里面Signing变成如下样子，Automatically manage signing 自动签名管理。报错了，之前一直使用的企业级开发者证书不可用了。

![img](https://github.com/mxdios/notebook/blob/master/notebooks/images/WechatIMG79.jpeg?raw=true)

Build Settings 里面的 Code Signing Identity 清一色选择为企业级开发者证书。还是报错

![img](https://github.com/mxdios/notebook/blob/master/notebooks/images/QQ20160914-0.png?raw=true)

![img](https://github.com/mxdios/notebook/blob/master/notebooks/images/QQ20160914-1.png?raw=true)

**解决办法：**

取消Automatically manage signing的选择，会出现两个Signing，分别对应Debug和Release，选择该项目对应使用的Provisioning Profiles即可

![img](https://github.com/mxdios/notebook/blob/master/notebooks/images/QQ20160914-2.png?raw=true)

## 注释快捷键command+/失效

Xcode8.0的注释// 生成快捷键 command+/失效了

**解决办法：**

终端运行命令：`sudo /usr/libexec/xpccachectl`
Password：`输入开机密码`
重启Mac生效

生效后空行快捷键，只在代码行能产生/解除注释，空行不能产生注释。记得之前可以来这....

## Xcode控制台输出问题

升级Xcode8.0之后运行项目，控制台疯狂打印了N多东西，完全看不懂啊，完犊子了，这么多需要适配的...

眼不见心不烦，下面就提供去掉这些乱七八糟打印内容的方法。

**解决办法：**

在`Edit Scheme...`下的Run -> Arguments -> Environment Variables 添加  `OS_ACTIVITY_MODE` = `disable`

具体见下图

![img](https://github.com/mxdios/notebook/blob/master/notebooks/images/QQ20160914-3.png?raw=true)







