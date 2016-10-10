# TestFlight测试应用程序

## 前言

app提交审核之前进行反复测试，以确保优质用户体验。Apple对iOS生态系统做了相当的布局完善，未越狱iPhone手机不能随便安装应用，AppStore上架的应用会经过严格审核(~~姑且称之为严格吧~~)，以确保iOS生态环境的良性发展。相比较来说虽然Mac系统还是蛮不错的，但是Mac的生态环境简直糟透了。

这种严苛的限制也带来了相应的弊端，其中之一就是iOS开发人员测试问题，虽然iOS9以后iOS开发者不需要交99美元即可进行真机测试。当测试机众多、测试和开发不在一起、测试是个小白等等问题时，测试又成了一个大问题。

可以花$299购买一个企业级账号（个人买不了，必须提交公司的邓白氏码），这样就可以打包app让未越狱手机随意下载安装了。同样，在iOS9以后需要手动去设置-通用-设备管理里信任打包的开发者证书，才能正常开启app。相信使用企业级证书分发测试的开发者们，曾无数次向人解释怎么才能顺利打开app。

## 发布TestFlight测试

在[iTunes Connect](https://itunesconnect.apple.com/)进行TestFlight分发测试，分为内部测试和外部测试。

内部测试 不需要审核，但是添加的内部测试人员必须先添加为iTunes Connect用户，必须添加用户的Apple ID，添加的用户会收到一封加入开发者计划的邮件，同意之后便成为该账户下的开发者。法务管理可以赋予他不同权限。最多可以将25名具备管理、App管理、开发人员、营销或法务职能的 iTunes Connect 用户添加为内部测试员。

外部测试 需要审核，最多可添加2000名测试人员。要求的电子邮箱不用必须为Apple ID。

app打包完成之后，通过Application Loader交付应用程序，在iTunes Connect中的TestFlight即可构建版本进行测试。

## 进行TestFlight测试

开发者发布内部测试邀请之后，内部测试员列表会显示已通知，内部测试员会收到内测邀请邮件。内测人员需要去AppStore下载[TestFlight应用](https://itunes.apple.com/cn/app/testflight/id899247664?mt=8)。

![img](https://github.com/mxdios/notebook/blob/master/notebooks/images/QQ20161010-1.png?raw=true)

内测邮件，点击邮件里的**Start Testing**按钮：

![img](https://github.com/mxdios/notebook/blob/master/notebooks/images/QQ20161010-2.png?raw=true)

如果内测人员没有下载TestFlight，可以点击网页里的`TestFlight from the App Store`去AppStore下载安装。安装完成的拷贝红框中的黑体字符串邀请码，然后打开TestFlight应用。

![img](https://github.com/mxdios/notebook/blob/master/notebooks/images/QQ20161010-3.png?raw=true)

这里的Apple ID必须是邀请你时填的Apple ID，也是收到上述邮件的邮箱地址。如果不是的话，需要去`设置 `-> `iTunes Store 与 App Store` -> 注销现有的Apple ID，登录收到邮件的Apple ID

![img](https://github.com/mxdios/notebook/blob/master/notebooks/images/QQ20161010-4.png?raw=true)

点击蓝色的`Redeem`按钮，把拷贝的字符串邀请码粘贴到输入框中，点击右上角`Redeem`按钮。

![img](https://github.com/mxdios/notebook/blob/master/notebooks/images/QQ20161010-5.png?raw=true)

会出现内部测试的app，点击安装即可。

![img](https://github.com/mxdios/notebook/blob/master/notebooks/images/QQ20161010-6.png?raw=true)

