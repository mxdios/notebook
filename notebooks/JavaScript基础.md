# JavaScript基础

## 简介

网景公司的Brendan Eich在两周之内设计出了JavaScript语言。ECMA组织制定了JavaScript的语言标准，被称为ECMAScript。

js代码可以写在网页的任何地方，一般会在html的`<head>`里，在`<script>`...`</script>` 之间写js代码，或者在单独的`.js`文件中写js代码，在html中引入该文件`<script src="xxx.js"></script>`

```JavaScript
<script>
    alert('Hello, world');
</script>
```

**JavaScript区分大小写**

## Chrome调试js

Chrome浏览器里的`开发者工具`可以查看页面源代码，并运行调试js。

![img](https://github.com/mxdios/notebook/blob/master/notebooks/images/QQ20161101-0.png?raw=true)

## 数据类型和运算符

### Number

js用Number表示整型和浮点型，除了和其他语言类似的整型、浮点型，js的Number还多了两个数值表示`NaN`和`Infinity`。

`NaN`就是`Not a Number`，当无法计算结果时用`NaN`表示
`Infinity`表示无限大，当数值超过了Number的表达范围，就用`Infinity`表示

Number可以直接进行四则运算：`+` `-` `*` `/` `%`

### 字符串

字符串是用`""`或`''`包含的任意文本。

### 比较运算符

除了常用的`>` `<` `>=` `<=` `==`，js多了一个`===`比较运算符。

`==`它会自动转换数据类型再比较，结果有时匪夷所思。**不建议使用**

`===`它不会转换数据类型，如果数据类型不一样，就返回`false`，如果一样，继续比较值。**建议使用**

```JavaScript
false == 0; // 结果true
false === 0; // 结果false
```
注意`NaN`，`NaN`与任何值都不相等，和自身也不相等

```JavaScript
NaN === NaN; // 结果false
```

可以用`isNaN()`函数判断：

```JavaScript
isNaN(NaN); // 结果true
```

浮点型运算结果的比较，浮点型在运算过程中会有误差，计算机也不能准确表达一个无限小数，比较两个浮点型的值只能通过计算差值的绝对值，然后判断绝对值是否小于某个临界值

```JavaScript
1 / 3 === (1 - 2 / 3)// 结果false
Math.abs(1 / 3 - (1 - 2 / 3)) < 0.0000001; // 结果true
```






