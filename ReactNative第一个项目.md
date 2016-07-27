# 第一个项目

**第一个ReactNative是网络获取电影信息，用列表展示。如图oc中基本的tableview样式**

**项目源码请见[firstReactNative](https://github.com/mxdios/firstReactNative)**

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

`Text` `View` 是一些常用的控件组件，`Image`组件添加到这下面，注意要在`View`后面写`,`(最后一个后面的`,`可写可不写)

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

## 改变样式排版

现在是文字在上图片在下的样式，需要调整样式，图片在左边，文字在右边。这是基本的tableview样式

在`styles`里添加几个样式

```
rightContainer: { //存放文字view的样式
	flex: 1, //是让里面存放的元素具有相同的宽度，忽略存放内容。css样式属性。
},
title: {//标题样式
	fontSize: 20,
	marginBottom: 8,
	textAlign: 'center',
},
year: {//简介样式
	textAlign: 'center',
},
```

修改`return ()`里面的代码，将新添加的样式添加到相应组件上

```
return (
  <View style={styles.container}>
  	<Image 
      source = {{uri: movie.posters.thumbnail}}
      style = {styles.thumbnail}
     />
    <View style = {styles.rightContainer}>
    	<Text style = {styles.title}>{movie.title}</Text>
    	<Text style = {styles.year}>{movie.year}</Text>
    </View>
  </View>
);
```
相比于之前代码，`<Image />`代码块移动到了`<Text>`上面，因为image要在文字的左边，从上到下，从左到右渲染，如果还放在text的下面的话，会出现文字在左边，图片在右边的效果，请自行试验。

把`Text`用一个`<View>`组件包起来，并赋予样式`rightContainer`，里面有一个`flex: 1`属性，这个容器的属性宽度会自动变宽，不会因为内容多少而定，会把图片挤压到屏幕边缘

给文字分别添加`title` `year`样式，指定的文字的大小以及居中显示。 `marginBottom: 8,`是让`title`下面有8像素的距离。

`command + R`刷新模拟器运行程序。

![image](http://oalg33nuc.bkt.clouddn.com/image/Simulator%20Screen%20Shot%202016%E5%B9%B47%E6%9C%8827%E6%97%A5%20%E4%B8%8A%E5%8D%889.34.57.png)

至此，基本的单个样式已完成，如图定义完成tableview里单个cell的样式。下面开始获取网络真是数据，完成列表展示

## 获取网络数据

从`Rotten Tomatoes（美国影评网站烂番茄）`的api获取数据

在文件开头`import`的下面定义接口地址变量

```
var REQUEST_URL = 'https://raw.githubusercontent.com/facebook/react-native/master/docs/MoviesExample.json';
```

把下面代码放到`render()`函数之前。在网络获取之前初始化一个null状态，根据`this.state.movies == null`判断数据是否抓取成功。

```
constructor(props) {
	super(props);
	this.state = {
		movies: null,
	};
	this.fetchData = this.fetchData.bind(this);
}
```

添加React组件生命周期方法`componentDidMount()`，在组件加载完毕之后执行，只执行一次，在这里进行数据请求。

```
componentDidMount() {
	this.fetchData();
}
```
写`fetchData()`函数，网络请求

```
fetchData() {
	fetch(REQUEST_URL)
		.then((response) => response.json())
		.then((responseData) => {
			this.setState({
				movies: responseData.movies,
			});
		})
		.done();
}
```

修改原来的render函数，修改样式

```
render() {
	if (!this.state.movies) { //如果没有数据 返回renderLoadingView()
		return this.renderLoadingView();
	}
	var movie = this.state.movies[0];
	return this.renderMovie(movie); //有数据的时候，执行这个函数
}
```

实现`renderLoadingView()`函数和`renderMovie()`函数

```
renderLoadingView() {//添加加载提示
	return (
		<View style = {styles.container}>
			<Text>
				正在加载电影数据...
			</Text>
		</View>
	);
}
renderMovie(movie) {//拿到网络获取的数据，添加到控件上
	return (
  	<View style={styles.container}>
  		<Image 
      		source = {{uri: movie.posters.thumbnail}}
      		style = {styles.thumbnail}
     	/>
    	<View style = {styles.rightContainer}>
    		<Text style = {styles.title}>{movie.title}</Text>
    		<Text style = {styles.year}>{movie.year}</Text>
    	</View>
  	</View>
	);
}
```

`command + R`刷新模拟器运行程序。这时候得到的数据是网络数据

![image](http://oalg33nuc.bkt.clouddn.com/image/Simulator%20Screen%20Shot%202016%E5%B9%B47%E6%9C%8827%E6%97%A5%20%E4%B8%8A%E5%8D%8810.34.16.png)

## ListView

`ListView`可以说是ReactNative里的`UITableView`，会安排视图的渲染，只渲染屏幕上显示的元素，渲染完毕被移出屏幕外的元素会被移除掉。

第一步引入`ListView`组件，跟上文中引入`Image`组件一样。

修改render函数，把返回一条数据的代码修改为渲染包含多个电影信息的`ListView`

```
render() {
	if (!this.state.loaded) {
		return this.renderLoadingView();
	}
	return (
		<ListView 
			dataSource = {this.state.dataSource}
			renderRow = {this.renderMovie}
			style = {styles.listView}
		/>
	);
}
```

修改`constructor(props)`代码，不再使用`this.state.movies`来存放数据，把数据存放到`dataSource`里面，可以根据布尔值`this.state.loaded`来判断是否加载完毕

```
constructor(props) {
	super(props);
	this.state = {
		dataSource: new ListView.DataSource({
			rowHasChanged: (row1, row2) => row1 !== row2,
		}),
		loaded: false,
	};
	this.fetchData = this.fetchData.bind(this);
}
```

修改`fetchData`方法，把数据更新到dataSource

```
fetchData() {
	fetch(REQUEST_URL)
		.then((response) => response.json())
		.then((responseData) => {
			this.setState({
				dataSource: this.state.dataSource.cloneWithRows(responseData.movies),
				loaded: true,
			});
		})
		.done();
}
```

添加样式`listView`

```
listView: {
  	paddingTop: 20,
  	backgroundColor: '#F5FCFF',
},
```

`command + R`刷新模拟器运行程序。达到我们最终要的结果

![image](http://oalg33nuc.bkt.clouddn.com/image/Simulator%20Screen%20Shot%202016%E5%B9%B47%E6%9C%8827%E6%97%A5%20%E4%B8%8A%E5%8D%8811.04.52.png)