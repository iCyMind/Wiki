# 几种滚动距离的区别

CSS/JS 里有若干个取得元素距离的方法以及属性:

- scrollTop
- scrollY
- pageYOffset
- offsetY
- offsetTop
- getBoundingClientRect()

## scrollY

顾名思义, 表示在 Y 轴上的滚动距离. 这是一个 window 上的只读属性. 它有一个 alias: pageYOffset

## scrollTop

如果一个元素会产生滚动条(默认情况下, 浏览器只有一个默认的垂直滚动条. window.scrollTop 可以取到这个滚动条的滚动距离), 那么这个表示已经滚动的距离.
默认情况下, 浏览器只有一个默认的垂直滚动条, 这个滚动条的滚动距离, 只能用 window.scrollY 取到.

如果一个元素限制了高度, 又设置了 overflow 相关属性, 那么用 element.scrollTop


## offsetTop

带 offset 的都比表示偏移量, offsetTop 表示一个元素的顶部, 到它最近的父级定位元素的顶部的偏移. 更准确地说是上边框上沿到父级上边框下沿的距离, 而且这个距离会计算他们之间的所有同级元素的空间, 就算把一个容器内的最后一个元素滚动到贴近容器顶部, 该元素的 offsetTop 和没滚动前的一样. 或许可以这么理解:

它把所有的同级元素平铺展开来计算距离

## offsetY

这个是鼠标事件相关的.

## getBoundingClientRect()

该方法返回一个 DOMRect 对象, DOMRect 的 top 属性表示, 该元素的顶部到视窗顶部的距离. 滚动会影响这个值, 可能会变成负数. 有意思的是, 这个 DOMRect 对象上的 top/left...等等属性, 是不可枚举/迭代的, 因为他们是根据其他属性计算出来的.
