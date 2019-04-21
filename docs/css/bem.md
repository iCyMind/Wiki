# Block, Element, Modifier


## 什么是块?
首先块肯定不是 CSS 概念里的块级元素, 一个块是一个独立的部分, 可以不依赖其他部分而存在. 而且可以复用?

一个块不可以影响他的外部环境, 所以不可以使用非 static 以外的定位方法.

疑惑: 如果不做定位, 没有margin, 那么怎么实现布局?



## CSS with BEM

### HTML for CSS
如何包一层 wrapper?

通常来说, wrapper 的作用是:
- 作用A: 相对其他元素来对本元素进行定位
- 作用B: 对 section 里的元素进行定位
以上两个作用, BEM 通过 mixes 或者通过创建额外的 block 元素来实现. 所以不需要创建额外的抽象 wrapper, 因为他无法提供任何功能.

- BEM 实现作用A
比如 `footer` 这个块要和其他元素隔开 `50px`. 首先一般状态下的 `footer` 不含padding, 这时候可以在 `footer` 上应用一个 mixes 来覆盖 padding:
```css
.page_footer {
  padding: 50px;
}
```

- BEM 实现作用B
比如 `footer` 和 `header` 需要指定宽度并垂直居中, 那么可以在他们外面包一层 `page__inner`, 然后应用相应的[规则](https://en.bem.info/methodology/css/#positioning-elements-inside-a-block):

但是这样一来, 不还是创建了一个 wrapper 吗? 只是这个 wrapper 利用 BEM 规则起了个名字罢了. 或者用 mixes 也可以实现这个目的呀?

原因可能在于: `page__inner` 已经是一个可复用的 BEM 实体了, 而抽象的 wrapper 则没这个作用. 同时它和 mixes 也是不同的

- Selectors
  - BEM 不使用标签选择符和 id 选择符
  - 使用类名来选择元素
  - 不推荐类名和标签名结合使用, 因为会导致优先级增加
  - 只在特定的情况下才使用嵌套的选择器: 当需要根据 block 或者 theme 来更改 element 的样式时
  - 不推荐使用诸如 `.button.button_theme_islands {}` 之类的联合选择器, 因为会导致优先级增加. 而应该分开写 `.button {} .button_theme_islands {}`
  - 名称需要符合 BEM 实体的[规则](https://en.bem.info/methodology/naming-convention/)

- Modifiers
block 的外观可以通过设置 modifier 或者通过移除 modifier 来改变. Modifiers 不是用来取代块的类名, 而是配合使用. 如 `.button_size_m` 不能单独使用, 通常在 html 里会写 `class="button button_size_m"`

- Mixes
  - External geometry and positioning: 如前面提到的作用A: 相对其他元素来对本元素进行定位
  - 给几个block添加相同样式
    如果利用诸如 `.article, .footer {}` 之类的组合选择器来应用样式的话, 虽然方便, 但是会增加耦合度. 可以利用 mix 来解耦, 把共同样式抽取到一个 `text` 类中, 然后在在 html 中指定元素都加上这个类即可.

- Dividing the code into parts
  - 把css代码分成好几块, 按照一定的[组织方法](https://en.bem.info/methodology/filestructure/)进行存放, 好处是方便浏览/方便复用/work with redifinition levels
  - 单一责任原则: 如果一个普遍的类 `.button` 定义了一组样式, 需要再某个场景下定义它的 padding/margin 之类的, 那么在新类 `.header__button` 中, 只做定位就好了, 不要再覆写其他属性.
  - 开放/闭合原则: 不要利用相同的类名覆写规则, 也不要根据上下文覆写规则, 而是利用 modifier 覆写规则?
  - DRY: 抽取相同部分, 不同的 Modifiers 覆写成不同的样式
  - composition instead of inheritance
- Dividing code by redefinition levels and building an assembly
- How to migrate from CSS to BEM
