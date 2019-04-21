# CSS

## 自动刷新工具

[browsersync](https://www.browsersync.io)

## IE8 和 HTML5标签

IE8 不认识html标签, 必须要[脚本](https://github.com/aFarkas/html5shiv)曲线救国:

```html
<!--[if lt IE 9]>
<script src="html5shiv.js"></script>
<![endif]-->
```

## 合适的html标签

如果不懂使用什么标签, [css3 the missing manual][css3 the missing manual] 的16页有很好的提示.

## 其实并没有CSS3的存在

CSS2.1之后, CSS被分割成了不同的模块, 比如Selector模块, Box Arrangement 模块等等, 每个模块独立开发.

## 选择器

### 伪元素和伪类

顾名思义.

CSS3里伪元素以双冒号开头, 但是IE8不支持双冒号开头的伪元素, 因此一直沿用单冒号也没事, 除了`::selection`伪元素, 它没有单冒号版本.

### 属性选择器

- 选择包含某属性的元素:`img[title]`
- 选择包含某属性且属性值等于x的元素: `img[title="x"]`
- `a[href^=""file://""]`
- `a[href$=".pdf"]`
- `img[src*="headshot"]`

### 直接后代选择器

和后代选择器 `div p` 不同, 直接后代选择器可以选择元素x的直接后代y:`div > p`

通过一些伪类, 还可以指定特定的后代:

- `:first-child`
- `:last-child`
- `:only-child`
- `:nth-child`

    <img src="nth-child.jpg" width="500">

- `:first-of-type`
- `:last-of-type`
- `:nth-of-type`

### 兄弟选择器

- `h2 + p`
- `h2 ~ p`

### target选择器

比如某个a元素链接到页内元素x, 那么x就是target元素. `:target`可以对x元素进行定制

### not选择器

not选择器的限制是:you can only use :not() once with a selector. 类似`a[href^="http://"]:not([href*="google.com"]):not([href="yahoo.com])`是不行的.像以下这么用是可以的:

- `p:not(.classy)`
- `a[href^="http://"]:not([href^="http://mysite.com"])`

### 单位

- ch: 指的是所使用的字体中0的高级宽度(advance width), 其中: a character glyph’s advance width is the distance from the start of a character glyph to the start of the next.

### 视觉可视化

- 替换元素和非可替换元素

非可替换元素是指内容包含在文档中的元素, 如p. 可替换元素指的是元素本身只是一个占位符, 真正的内容另有其人, 如img元素.

img 作为可替换元素, 同时一个一个inline元素, 但是表现上可以设置宽高, 更像一个inline-block.

- 疑问: img的包含块是什么? 书上说, 七大金刚加起来等于包含块的宽度, 对于img来说这是什么意思. 为什么img的可以大过包含它的div

### 块级格式化上下文

[BFC详解](https://getpocket.com/a/read/567841622)

### 居中元素

[Centering in CSS: A Complete Guide](https://css-tricks.com/centering-css-complete-guide/)
### flex 布局

[Flex 布局教程: 语法篇](https://getpocket.com/a/read/977475539)
[Flex 布局教程: 实例篇](http://www.ruanyifeng.com/blog/2015/07/flex-examples.html)

[css3 the missing manual]: file:///Users/Simon/Dropbox/Books/CSS3%20The%20Missing%20Manual%2C%204th%20Edition.pdf
