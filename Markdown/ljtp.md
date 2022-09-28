## 插入链接与插入图片

**插入链接与插入图片的语法很像，区别在一个 `!` 号**

```
链接：[链接标题文字](URL地址)
[链接标题文字](URL地址)
<链接地址>

图像：![图像描述文字](URL地址)

示例链接：
更多教程尽在：[某某博客](https://www.baidu.com/)
[某某博客](https://www.baidu.com/)
<https://www.baidu.com>

示例图片：
![头像](https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fc-ssl.duitang.com%2Fuploads%2Fitem%2F202006%2F21%2F20200621031631_lggnn.thumb.400_0.png&refer=http%3A%2F%2Fc-ssl.duitang.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=auto?sec=1666628636&t=a52e93fcdb11c4c8a0b59e6caeae74a5)
```

链接效果：

更多教程尽在：[某某博客](https://www.baidu.com/)

[某某博客](https://www.baidu.com/)

<https://www.baidu.com>

图片效果：

![头像](https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fc-ssl.duitang.com%2Fuploads%2Fitem%2F202006%2F21%2F20200621031631_lggnn.thumb.400_0.png&refer=http%3A%2F%2Fc-ssl.duitang.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=auto?sec=1666628636&t=a52e93fcdb11c4c8a0b59e6caeae74a5)



**内联链接**链接和图像使用方式相同

创建内联链接，将链接文字包含在 **方括号 `[]`** 中，并紧跟着 **小括号 `()`** 。在括号内，指定链接的URL，可选地，**`空格`** 后面跟着链接的 **title属性**，用 **双引号**（推荐）、**单引号** 或 小括号 包围起来。

```
示例：
更多教程可去：[首页](/)查看
[首页](/)

示例图片：
![头像](tp/ljtp.tp/2.png)
```

更多教程可去：[首页](/)查看

[首页](/)



##  控制图片大小与居中

为适配兼容其它 Markdown编辑器，这里使用HTML `<img>` 等标签

```
<img src="图片路径" width="图片宽度" height="图片高度"/>

居中显示+大小：
<p style="text-align: center;">
    <img src="图片路径" width="图片宽度" height="图片高度"/>
</p>

示例：
<img src="https://gimg2.baidu.com/1.png" width="10px" height="10px"/>

居中显示+大小：
<p style="text-align: center;">
    <img src="https://gimg2.baidu.com//1.png" width="10px" height="10px"/>
</p>

```

效果：

<img src="https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fc-ssl.duitang.com%2Fuploads%2Fitem%2F202006%2F21%2F20200621031631_lggnn.thumb.400_0.png&refer=http%3A%2F%2Fc-ssl.duitang.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=auto?sec=1666628636&t=a52e93fcdb11c4c8a0b59e6caeae74a5" width="300px" height="250px"/>

居中显示+大小：
<p style="text-align: center;">
    <img src="https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fc-ssl.duitang.com%2Fuploads%2Fitem%2F202006%2F21%2F20200621031631_lggnn.thumb.400_0.png&refer=http%3A%2F%2Fc-ssl.duitang.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=auto?sec=1666628636&t=a52e93fcdb11c4c8a0b59e6caeae74a5" width="300px" height="250px"/>
</p>

