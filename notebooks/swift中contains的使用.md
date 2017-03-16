`contains`是swift中的一个实例方法，返回一个Bool值，用于做“是否包含”的判断。简单的说就是：告诉你一堆玩意儿中是否有你要的玩意儿。

## 方法contains()

用法1：判断字符串中是否包含某个字符或某段字符串

```swift
let str = "abcdef"
print(str.contains("a"))//true
print(str.contains("bcd"))//true
print(str.contains("g"))//false
```

用法2：判断数组中是否包含某个元素

```swift
let name = ["Mark", "Kim", "Goudan"]
print(str.contains("Mark"))//true
print(str.contains("MJ"))//false
```

## 方法contains(where: )

该方法接收一个闭包，以序列元素作为闭包的参数，返回一个Bool值，判断该序列中是否有元素符合闭包条件。

用法1：判断Int数组中元素有没有大于100的

```swift
let list = [11,22,44,66,88,100]
let zz = list.contains(where: { (value) -> Bool in
    return value > 100
})
print(zz)//false
```

闭包的写法可以简化，闭包中只有一行表达式时，可以省略return，直接写表达式，隐式返回：

```swift
let zz = list.contains(where: { (value) -> Bool in
	 value > 100
})
```

闭包的参数名可以简化为$0，表示第i个参数：

```swift
let zz = list.contains(where: {$0 > 100})
```

用法2：判断对象数组中对象元素的某个key所对应的value是存在某值

例如：判断下面`dictList`数组中的字典元素里面是否有`"name"` == `"Mark"`的元素存在

```swift
let dictList = [["name": "Goudan", "age": "22"], ["name": "Mark", "age": "18"], ["name": "Maoya", "age": "19"], ["name": "Tuzi", "age": "25"], ]
```

当然可以通过`for`循环遍历`dictList`数组，挨个字典元素检测。这样做太麻烦，可以使用`contains(where: )`方法，传入一个闭包，完成检测

```swift
let isMark = dictList.contains(where: { (dict:[String : String]) -> Bool in
    dict["name"] == "Mark"
})
```

可简写为：

```swift
let isMark = a.contains(where: {$0["name"] == "Mark"})
```


