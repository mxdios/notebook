# AutoLayout和SizeClass要点笔记

## 添加约束Add new Constraints

给一个控件添加约束，指定上下左右距离相邻控件是否固定距离。如果没有相邻控件，就是与父控件边缘的距离。

UILabel等控件可以根据内容自动调节控件大小，如果控件在storyboard中拖大了，添加距离约束后会有黄色警告，点击警告选择`update frame`，会恢复自动调节大小。如果想保留固定宽度，在width和height写入固定宽度即可。

xcode6以后有一个新特性，添加约束时，默认在添加的约束基础上增加20px，只需要在添加约束时，去掉勾选`constraints to margins`即不会再增加。

`Identity Inspector`里的`Document`设置`Label`属性是该控件的名字显示，没多大卵用。

添加居中约束时，控件必须有宽高，有宽高才能来计算居中。但是类似UILabel、UIImageView等根据内容自适应宽高的，就必须要么有内容要么给定宽高，不然约束报错。





