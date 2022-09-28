**设计封面**

封面需要在根目录的`_coverpage.md`文件中配置，例如`docsify`官方文档的封面内容如下：

```
_coverpage.md<!-- _coverpage.md -->

![logo](https://throwable-blog-1256189093.cos.ap-guangzhou.myqcloud.com/202009/_media/icon.svg)

# docsify <small>3.5</small>

> 一个神奇的文档网站生成器。

- 简单、轻便 (压缩后 ~21kB)
- 无需生成 html 文件
- 众多主题

[GitHub](https://github.com/docsifyjs/docsify/)
[Get Started](#docsify)
```

渲染效果如下：

![image-20220924135448573](tp/fm.tp/image-20220924135448573.png)

笔者也参照此配置做了一个丑丑的主页，内容如下：

```
_coverpage.md![logo](https://throwable-blog-1256189093.cos.ap-guangzhou.myqcloud.com/202009//leaf.svg)

# Spring Album <small>0.0.1</small>

> 试下写个Spring相关的专栏，这是原始版本，暂定包括下面的栏目：

- `SpringBoot2.x`入门系列 
- `SpringBoot2.x`进阶和实战
- `SpringBoot`源码系列

[GitHub](https://github.com/zjcscut/spring-boot-guide)
[Get Started](#Spring)
```

渲染效果如下：

![image-20220924135603686](tp/fm.tp/image-20220924135603686.png)

封面的内容可以使用`html`或者`markdown`语法编写，自由度极高。

> 封面的背景颜色是随机切换的，可以使用`![color](#f0f0f0)`设置固定的背景色



**自定义背景**

目前的背景是随机生成的渐变色，我们自定义背景色或者背景图。在文档末尾用添加图片的 Markdown 语法设置背景。

```
_coverpage.md_coverpage.md
<!-- _coverpage.md -->

# docsify <small>3.5</small>

[GitHub](https://github.com/docsifyjs/docsify/)
[Get Started](#quick-start)

<!-- 背景图片 -->

![](_media/bg.png)

<!-- 背景色 -->

![color](#f0f0f0)
```