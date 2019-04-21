# 浮动, 外边距, 以及 BFC

都2018年了, 实现三栏布局肯定是用 flex 方便了. 理解圣杯布局和双飞翼布局对理解浮动,负外边距以及BFC 很有好处.

## 三栏布局的题外之意

三栏布局, 除了左右边栏固定宽度, 中间内容自适应之外. 其实应该还有几个隐含的要求:

- 内容要优先展示
- 还有一个底栏/顶栏
- 三栏的高度不一致时可以, 底栏在最高的板块的下方
- 父容器不能发生高度塌陷

基于以上几点要求, 很多三栏的布局方案就已经被毙掉了:

- 绝对定位 => 父容器高度无法确定
- 左右浮动 => 内容无法优先渲染

## DOM 结构

在 DOM 结构已经固定为如下情形:

```html
<!-- 结构1 -->
<header></header>
<div class="container">
  <div class="content"></div>
  <div class="left"></div>
  <div class="right"></div>
</div>
<footer></footer>


<!-- 结构2 -->
<header></header>
<div class="container">
  <div class="wrapper">
    <div class="wrapper-content"></div>
  </div>
  <div class="left"></div>
  <div class="right"></div>
</div>
<footer></footer>
```

基本的 css:

```css
* {
  opacity: .8;
}
header, footer {
  height: 30px;
  background-color: teal;
}
.container {
  background-color: #222;
}
.left {
  width: 40px;
  height: 300px;
  background-color: green;
}
.content {
  height: 200px;
  background-color: red;
}
.right {
  width: 60px;
  height: 100px;
  background-color: blue;
}
```

初始状态如图: ![init][init]

## flex 碾压一切

```css
.container {
  display: flex;
  flex-direction: row;
}
.left, .right {
  flex-shrink: 0;
  flex-grow: 0;
}
.left {
  order: -1;
}
.content {
  flex-grow: 1;
}
```

flex 效果: ![flex][flex]


## 圣杯

除了 flex, 我们可以用浮动来布局. 要想 left, right, content 的顶端能够对齐, 有几种方法.

1. 用绝对定位, 但是这样一来 container 的高度就没法自适应了. 毙掉
2. left 用相对定位, 但是 content 的高度不确定, 不知道该把  left 向上移动多少. 毙掉.
3. 让 content 脱离文档流, left 自动顶替 content 占用的位置. left/content 顶部对齐了, 但是 right 不管左浮动还是右浮动都没法对齐.
4. 所以 left 也要 float, content 占满了宽度, left 浮不上去. 这里有个有趣的特点: 如果一个浮动元素被另外一个浮动元素挤到了屏幕下方, 那么可以对第二个元素设置一个负 margin, 这样他可以上升到同一水平.





首先把三个 div 都浮动:
```css
.content, .left, .right {
  float: left;
}
```
![float-init][float-init]

什么鬼?!

- 子元素全浮动后, 容器高度坍塌了
- div 没有指定宽度的话, 默认占满父容器的宽度. 但是一旦进行了浮动, 宽度变成他的内容的固有宽度. content 没有内容, 所以宽度为0.

我们让容器生成 BFC, 因为 BFC 内, 浮动元素也参与高度的计算, 所以可以解决高度坍塌的问题. 再指定 content 宽度为 100%, 此处百分比表示父容器的内容区(不包括 padding 和 margin)的比值. 这个百分比不是不是基于 width 值, 而是内容区域的真实宽度, 如果父容器有个负左margin, 那么真实宽度要比 width 值大.

```css
.content, .left, .right {
  float: left;
}
.container {
  overflow: hidden;
}
.content {
  width: 100%;
}
```
![float-bfc][float-bfc]



[init]: ./float-margin-and-bfc/init.png
[flex]: ./float-margin-and-bfc/flex.png
[float-init]: ./float-margin-and-bfc/float-init.png
[float-bfc]: ./float-margin-and-bfc/float-bfc.png

