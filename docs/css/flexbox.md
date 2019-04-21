# flexbox

![flexbox](flexbox/flexbox.png)

利用 flexbox 可以制作两边栏固定宽度中间栏占满空间的三栏布局:

```html
<div class="container">
    <div class="left">left</div>
    <div class="main">main</div>
    <div class="right">right</div>
</div>
```

```css
.container {
    display: flex;
}
.left, .right {
    flex-shrink: 0;
    background-color: lightblue;
    width: 120px;
}
.main {
    flex-grow: 1;
    background-color: teal;
}
```

其中有两个关键点:

1. .main 的 flex-grow 需要设置为1. 表示当 flex-items 的宽度小于容器的宽度时, .main 占满剩余的容器宽度
2. .left,.right 的 flex-shrink 需要设置为0, 表示当 flex-items 的宽度大于容器的宽度时, .left,.right不缩放.

如果只设置 .main 的 flex-grow, 那么.main 的宽度很大时(比如一行很长的句子), 会导致 items 的宽度大于容器宽度. 那么因为 flex-shrink 的默认值是1, 这就导致了 .left/.right 的宽度会缩小.
