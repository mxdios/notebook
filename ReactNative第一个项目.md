# 第一个项目

**第一个ReactNative是网络获取电影信息，用列表展示。**

展示我的学写该项目的学习笔记，掺杂了一些我的个人理解，可能会更清楚一些。[原文地址](https://github.com/reactnativecn/react-native-docs-cn/blob/master/docs/0.28/sample-application-movies.md)

## 创建项目

创建第一个项目，项目名字为`firstReactNative`

```
react-native init firstReactNative
```

用终端或Xcode运行该项目，具体步骤见[React Native准备工作](https://github.com/mxdios/notebook/blob/master/ReactNative%E5%87%86%E5%A4%87%E5%B7%A5%E4%BD%9C.md)

![如图](http://oalg33nuc.bkt.clouddn.com/image/rn1.png)

## 引入组件

创建完毕项目打开`index.ios.js`文件，文件上部有如下代码

```
import React, { Component } from 'react';
import {
  AppRegistry,
  StyleSheet,
  Text,
  View
} 
```
这里是React Native中的一些组件，需要引入。项目中牵扯到图片展示，需要引入`Image`组件。

顾名思义第一个`import`是引入组件框架，第二个`import`是引入几个常用的组件。

`AppRegistry` 是注册应用组件，应用只有注册之后才能正确渲染，应用只整体注册一次，并不是每个组件都注册，对应该文件最下面的注册应用操作

```
AppRegistry.registerComponent('firstReactNative', () => firstReactNative);
```

`StyleSheet` 是样式创建组件，在`const styles = StyleSheet.create()`使用。

`Text` `View` 是一些常用的控件组件，`Image`组件添加到这下面，注意要在`View`后面写`,`，(最后一个后面的`,`可写可不写)

```
import React, { Component } from 'react';
import {
  AppRegistry,
  StyleSheet,
  Text,
  View,
  Image,
} 
```

## 创建模拟数据

在任意位置添加变量数据`URLDATA` (不要在其他方法{}内添加)

```
var URLDATA = [
	{title: '标题', year: '2016', posters:{thumbnail: 'http://i.imgur.com/UePbdph.jpg'}}
];
```

iOS9中http请求需要添加允许任意的请求方式，在`info.plist`里添加`App Transport Security Settings`在该字段下选择`Allow Arbitrary Loads`并设置为`YES`。原因见[iOS9的适配特性](https://github.com/mxdios/notebook/blob/master/ios9%E7%9A%84%E9%80%82%E9%85%8D%E7%89%B9%E6%80%A7.md)

## 修改组件

### 渲染组件

在`class firstReactNative extends Component {` 里面更改为如下代码：

```
render() {
    var movie = URLDATA[0]
    return (
      <View style={styles.container}>
        <Text >{movie.title}</Text>
        <Text>{movie.year}</Text>
        <Image source = {{uri: movie.posters.thumbnail}} />
      </View>
    );
}
```

`movie`是拿到的模拟数据分别用`Text` `Image`渲染到屏幕上。`command + R`刷新模拟器运行程序。这时候还看不到图片，因为没有指定图片的宽高

![如图](http://oalg33nuc.bkt.clouddn.com/image/rn2.png)

### 设置组件样式

把原有代码块`const styles = StyleSheet.create({});`删除掉

创建新的渲染样式

```
var styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
    backgroundColor: '#F5FCFF',
  },
  thumbnail: {
    width: 53,
    height: 81,
  },
});
```

并把图片的渲染样式`thumbnail`添加到组件`Image`上

```
<Image 
	source = {{uri: movie.posters.thumbnail}}
	style = {styles.thumbnail}
/>
```

`command + R`刷新模拟器运行程序。

![如图](http://oalg33nuc.bkt.clouddn.com/image/rn3.png)