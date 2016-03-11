# 简介

## HTML5 文档类型

HTML5 文档类型声明相当简洁。<!DOCTYPE html>  

保留文档声明，主要由于历史原因。如果没有文档类型声明，那么大多数浏览器将会转换到一种混杂模式（quirk mode）。不同浏览器的混杂模式也不一样出现各种不一致问题。  
而添加文档类型声明后，浏览器会开启标准模式（standard mode），所有现代浏览器都以一致的格式和布局显示网页。

# 构造网页的新方式

## 语义元素

HTML5 中新的语义元素可以为它们标注的内容赋予额外的含义。  

比如：  

`<time>` 标注一个有效的日期和时间。
`<nav>` 标识一组导航链接。  
`<footer>` 标识页脚。

虽然这些语义元素并没有作任何事，但是仍有以下优点：

- 容易修改和维护
- 无障碍性
- 搜索引擎优化
- 未来的功能

正确地使用语义元素，就能创建清晰的页面结构，能够适应未来浏览器和 Web 设计工具的发展趋势。

比如如下一篇博客的 HTML5 排版

```
<article>
    <header>
        <h1>How the World Could End</h1>
    </header>
    
    <div class="Content">
        ...
    </div>
    
    <footer>
        ...
    </footer>
</article>
```

## 用`<figure`>添加插图

```
<figure class="FloatFigure">
    <img src="human_skull.jpg">
    <figcaption>Will you be the last person standing if one...</figcaption>
</figure>
```

## 用`<aside>`添加附注

## 浏览器对语义元素的支持

浏览器会把不认识的元素当成内联（inline）元素的习惯。  

不认识 HTML5 语义元素的浏览器应该把它们显示为块级元素，为解决这个问题，可以在 CSS 中添加代码：

```
aritcle, aside, figure, figcaption, footer, header, hgroup, nav, section \,
summary {
    display: block;
}
```
但是在 IE8 之前的浏览器，会拒绝给无法识别的元素应用样式。我们可以通过 JS 创建新元素，让浏览器识别为外来元素：

```
<script>
    document.createElement('header');
</script>
```

为了避免不必要地加载 JS 文件，可以如下引用脚本：

```
<!--[if lt IE 9]>
    <script src="..."></script>
<![endif]-->
```
## 理解`<header>`

对于`<header>`元素来说，有两种使用方式。

- 标注内容的标题
- 标注网页的页眉

如果你处理的是内容，一般不必使用`<header>`。只有在内容标题还附带其他信息的情况下才使用`<header>`。    
如果你的内容只有一个标题，那就用一个`<h1>`。  
如果还有副标题，那就使用`<hgroup>`。  

## 用`<nav>`标注导航链接


