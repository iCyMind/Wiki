# css puzzle

## 一个 div 里若干高低不一致的 inline / inline-block 元素, 最后一个元素设置 `vertical-align: baseline;`, 那它对齐的是哪个兄弟元素的 baseline?

## 如何改变一个 div 的 baseline?

## 为什么匿名行内元素不能和 span 对齐?

```
<div id="app">
    XXxXX
    <span class="text">XXxXX冲天香阵透长安满城尽带黄金甲</span>
</div>
<style>
#app {
    font-size: 40px;
}
.text {
    font-size: 80px;
    vertical-align: middle;
}
</style>
```

## 如何理解 em box

- 如何查看 embox 的尺寸?

## 如何理解 content box

- 张鑫旭说 contentbox 就是 IE/Firefox 中文本选中的背景色区域, 我觉得是不对的. 因为选中文本背景色会把所有字形都囊括在内, 但是 contentbox 并不是字形组成的区域, 而是 embox 组成的区域?
- content box 就是一个个 embox 连到一起形成的区域.

## font-size 指定的是什么的大小, 为什么相同的 font-size, 不同的字体, 结果的尺寸有差别

对于某个 font-size 的渲染结果, 实际上是受字体的设计者控制的. font-size 指定的是 embox 的大小, 但是不同的字体字形和 embox 的关系可能都不一样. 有的字体所有字形尺寸和 embox 一毛一样, 有的字体字形尺寸都限制在 embox 的尺寸内, 有的字体, 一部分字形比 embox 小, 一部分字形比 embox 大. 可以参考 css the definitive guide 第五章 fonts 中 font-size 一节的图.


## 为什么 span 的背景色可以跑到父元素 p 的边框上？

[demo](https://codepen.io/icymind/pen/VxERBd)
