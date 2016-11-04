# Charles安装与使用

![img](https://github.com/mxdios/notebook/blob/master/notebooks/images/QQ20161104-0.png?raw=true)

Charles是常用的抓包工具，仿写app获取网络数据最有效的途径。

Charles是付费软件，免费使用30天。超过30天还可以使用，只是每次启动会有10s的延迟，4.0.1版本每隔一段时间会弹出Charles介绍页5s，消失后可继续使用，超过30分钟会弹出警告闪退，重启后即可使用。还是给不付费用户留了条坎坷的活路。

![img](https://github.com/mxdios/notebook/blob/master/notebooks/images/QQ20161104-10.png?raw=true)


## 安装

本文使用的是4.0.1版本，[Charles官网下载地址](https://www.charlesproxy.com/download/)，Charles支持Windows、Mac、Linux，找到对应系统下载安装即可。

## 设置系统代理

将Charles设置为系统代理服务器后才能完成抓包操作。安装完成后第一次启动，Charles会请求设置系统代理权限，输入系统登录密码授权。也可以勾选`菜单 -> Proxy -> macOS Proxy`设置为系统代理。

![img](https://github.com/mxdios/notebook/blob/master/notebooks/images/QQ20161104-1.png?raw=true)

设置完成之后，点击网页，会有网络请求出现在Charles界面。这就是截取本地的网络访问，也可以截取其他设备上的网络访问，比如iPhone上的app数据抓包，需要进行下面的设置。

## 钥匙串里信任根证书

点击`菜单 -> Help -> SSL Proxying -> Install Charles Root Certificate`，弹出钥匙串，也可以直接手动找到钥匙串打开。

![img](https://github.com/mxdios/notebook/blob/master/notebooks/images/QQ20161104-2.png?raw=true)

发现钥匙串里Charles证书是不受信任的，选中证书，右键 -> "显示简介"，点信任前的三角号，展开信任（默认收起来的），选择`始终信任`。

![img](https://github.com/mxdios/notebook/blob/master/notebooks/images/QQ20161104-3.png?raw=true)

设置完成之后，Charles根证书成为受此账户信任。

![img](https://github.com/mxdios/notebook/blob/master/notebooks/images/QQ20161104-4.png?raw=true)

## 设置Charles配置

需要打开Charles的代理功能，点击`菜单 -> Proxy -> Proxy Settings`，写入代理端口`8888`，勾选`Enable transparent HTTP proxying`，点击OK，如下图所示。

![img](https://github.com/mxdios/notebook/blob/master/notebooks/images/QQ20161104-5.png?raw=true)

## 查看电脑ip，并在iPhone上设置http代理

点击`菜单 -> Help -> Local IP Address`，可以显示当前电脑运行的ip地址。

![img](https://github.com/mxdios/notebook/blob/master/notebooks/images/QQ20161104-6.png?raw=true)

在iPhone中`设置 -> 无线局域网 -> 连接无线网的详情键(ⓘ)`，将HTTP代理选择为`手动`，服务器输入刚才查看的电脑ip，端口输入刚才设置的`8888`。

![img](https://github.com/mxdios/notebook/blob/master/notebooks/images/QQ20161104-7.png?raw=true)

打开iPhone上的app，Charles会出现连接请求，点击`Allow`完成连接即可。

![img](https://github.com/mxdios/notebook/blob/master/notebooks/images/QQ20161104-8.png?raw=true)

这时浏览app便能截取app内http请求的数据，下图是访问猫眼app截取到的首页数据。

![img](https://github.com/mxdios/notebook/blob/master/notebooks/images/QQ20161104-9.png?raw=true)

要截取手机上的https请求的数据，还需要在手机上安装证书。

## iPhone上安装证书，截取https数据

完成上文设置，iPhone中用Safari访问<http://charlesproxy.com/getssl>，会自动跳转到证书安装页，点击右上角安装 -> 输入手机密码 -> 点击右上角安装，即可安装完成。

| ![img](https://github.com/mxdios/notebook/blob/master/notebooks/images/QQ20161104-11.png?raw=true)| ![img](https://github.com/mxdios/notebook/blob/master/notebooks/images/QQ20161104-12.png?raw=true)|
|:-------------:|:-------------:|

如果弹不出证书安装界面，可以Safari访问<https://www.charlesproxy.com/documentation/additional/legacy-ssl-proxying/>，点击()前面的`here`即可。

![img](https://github.com/mxdios/notebook/blob/master/notebooks/images/QQ20161104-13.png?raw=true)

安装完成之后即可对https请求做数据抓包。

## 查看接口地址以及提交的参数

点击Charles工具栏上`笔`的按钮，会出现该条数据的接口地址和请求参数，下图是访问猫眼app的首页数据提交的接口地址和请求参数。

![img](https://github.com/mxdios/notebook/blob/master/notebooks/images/QQ20161104-14.png?raw=true)

## tips

用Charles抓包app数据完毕，关闭Charles后，记得将手机的无线局域网上的http代理关闭，不然手机就连不上网了。

