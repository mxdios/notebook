# React Native准备工作

## 装环境

安装`Homebrew`。mac的套件管理器

```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```
安装`Node.js`

```
brew install node
```

安装`react-native-cli`。 React Native的命令行工具

```
npm install -g react-native-cli
```


安装`Watchman`。Facebook提供的监视文件系统变更工具

```
brew install watchman
```	

## 创建React Native项目

定位的指定文件夹，创建React Native项目

```
react-native init firstReactNative
```

终端内运行项目

```
//进入到项目文件夹中
cd firstReactNative 
//运行项目
react-native run-ios
```

也可以在Xcode中点击`firstReactNative/ios/firstRectNative.xcodeproj`运行

## 编写代码并运行

用编译器打开`index.ios.js`文件，修改相应代码

在iOS模拟器上`command + R`刷新运行app