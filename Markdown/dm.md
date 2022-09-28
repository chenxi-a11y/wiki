## 代码片 (内联代码块)

如果是段落上的一个函数或片段的代码可以用可使用 **1个反引号 `** 将其包起来。反引号中的文本不会被格式化。

```
`printf` 函数
```

显示结果如下：

`printf` 函数

## 代码块 (围栏代码块)

引用代码时，如果引用的语句为多行，可在代码行首缩进 **4个空格** 或 **1个制表符 (**Tab 键**)** ，也可以使用 **3个反引号 `** 置于代码的首行和末行。

```
 	An highlighted block
 	var foo = "bar";
```

效果：

 	An highlighted block
 	var foo = "bar";

**你也可以用 ``` 包裹一段代码，并指定一种语言（也可以不指定）：**

````
```
$(document).ready(function () {
    alert('RUNOOB');
});
```
````

效果：

```
$(document).ready(function () {
    alert('RUNOOB');
});
```

### 语法高亮

语法高亮：要将代码或文本格式化为各自的不同块，可使用 **3个反引号 `** 包裹一段代码，并在**首行反引号后面指明 `语言标识符`**。

````
```[语言标识符]
  在这里插入代码
```
例如：
```javascript
  // An highlighted block
  var foo = "bar";
```
````

效果：

```javascript
  // An highlighted block
  var foo = "bar";
```