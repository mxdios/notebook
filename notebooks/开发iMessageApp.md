# 开发iMessage App

## iMessage App

iOS10、Xcode8.0发布之后，AppStore上出现了一类新的app —— iMessage App

一些椭圆形的app图标在AppStore上格外另类，这些app都显示`仅用于iMessage`，还有一些传统的app显示`提供iMessage app`。

Apple为自己的软件帝国又开辟了一条新的道路。在社交软件如洪水泛滥的今天，不少人已经几乎忘记了短信这种功能的存在，短信功能的用途恐怕只剩下收验证码、收账单信息了。

Apple此举不知能不能重振短信息的昔日雄风。

## iMessage AppStore

iMessage App存在于iOS的AppStore中，但是iMessage app有一个独立完整的AppStore，这个AppStore完全独立于iOS的AppStore，可以在Message（信息）中访问，只显示iMessage App。

# 做一款短信斗图的iMessage App

## 普通表情包

Xcode提供了`Sticker Pack Application`，这是一个创建表情包的模板，没有任何代码。

在Xcode中创建一个新的工程，选择iOS -> Application -> Sticker Pack Application

![img](https://github.com/mxdios/notebook/blob/master/notebooks/images/QQ20160921-1.png?raw=true)

创建完项目之后，发现只有一个Stickers.xcstickers，这里面有一个iMessage App Icon（app图标） 和 Sticker Pack（表情包）

找一些图片，直接拖动到Sticker Pack里面，我直接拖动的发过来的qq表情。

![img](https://github.com/mxdios/notebook/blob/master/notebooks/images/QQ20160921-2.png?raw=true)

在Xcode的右边，Sticker Pack设置尺寸的地方，这里的三个尺寸是表情的默认大小，在程序运行的时候，表情分别以不同的方式排布。

![img](https://github.com/mxdios/notebook/blob/master/notebooks/images/QQ20160921-7.png?raw=true)

拖完想要的表情，设置完尺寸之后，command + R运行程序。

|Small|Medium|Large|
|:---:|:---:|:---:|
|![img](https://github.com/mxdios/notebook/blob/master/notebooks/images/QQ20160921-3.png?raw=true)|![img](https://github.com/mxdios/notebook/blob/master/notebooks/images/QQ20160921-4.png?raw=true)|![img](https://github.com/mxdios/notebook/blob/master/notebooks/images/QQ20160921-6.png?raw=true)|

表情的三个尺寸`Small`、`Medium`、`Large`。apple对这三种尺寸并没有严格限制，只有一些建议的大小：

```
1. Small: 100 x 100 pt @3x scale (300 x 300 pixel image)
2. Medium: 136 x 136 pt @3x scale (378 x 378 pixel image)
3. Large: 206 x 206 pt @3x scale (618 x 618 pixel image)
```

对于图片的大小也有一些限制：

```
1. 文件中images不可以大于500kb;
2. iamge不可以小于100 x 100 pt (300 x 300 pixels).
3. iamge不可以大于206 x 206 pt (618 x 618 pixels).
4. 图片格式必须是 PNG, APNG, JPEG, GIF ;
```
太大的图片可以放到Sticker Pack包里，也能启动运行到程序里，只是无法点击发布，可以找个大图试一下。

## 动图表情包

图标格式支持GIF，创建动态表情包可以直接拖GIF图标。

另一种创建动态表情的方式是使用`Sticker Sequence`，序列存储帧动画的每一帧，组成一组动画。

在Sticker Pack空白处右键，Add Assets -> New Sticker Sequence

![img](https://github.com/mxdios/notebook/blob/master/notebooks/images/QQ20160921-5.png?raw=true)

将帧动画的每一帧图片拖动到`Frame1`、`Frame2`....`FrameN`中。添加完毕之后，commit + R运行程序即可

|添加帧图片|运行结果 帧动画/GIF|
|:---:|:---:|
|![img](https://github.com/mxdios/notebook/blob/master/notebooks/images/QQ20160921-8.png?raw=true)|![img](https://github.com/mxdios/notebook/blob/master/notebooks/images/QQ20160921-9.gif?raw=true)|

用`Sticker Pack Application`可以快速创建一个用于iMessage的表情包，不需要写任何代码。

# 开发一款真正的iMessage Application


