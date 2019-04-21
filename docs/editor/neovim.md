# neovim

## 无插件启动

`nvim -u NONE`

## 缺点
通过 ! 执行外部命令时, 没法交互. 这是为了保持 UI 上的一致做的妥协.

## 插件

### surround

可以为单词/句子/段落/选取添加外围的括号/标签

- 增加 surround: `ysiw'`, `ysaW'`, `ys$'`, `yss"`, `ySsB`, 大写 S 会把添加的符号放在单独的一行
- 改变 surround: `cs'"`
- 删除 surround: `ds'`
- b,B,r,a =),},],>

### deoplete

前提条件需要安装 python 插件: `pip3 install --user neovim`

如果需要 ruby 的补全, 还需要安装 ruby 插件: `gem install --user-install neovim`

### tagbar

tagbar 可以为 markdown 文件生成导航, 只要按照它的 [wiki](https://github.com/majutsushi/tagbar/wiki#markdown) 正确设置 ~/.ctags, 但是 ctags 无法正确处理层级, 它会把所有的 h2 放一起, 所有的 h3 放一起.

[markdown2ctags](https://github.com/jszakmeister/markdown2ctags)可以解决这个问题

tagbar 还可以显示 js 的代码导航, 按照 [wiki](https://github.com/majutsushi/tagbar/wiki#jsctags-depends-on-tern--recommended-) 的说明, 需要安装:

- tern_for_vim(插件)
- jsctags(全局)
- 安装完毕不需要额外设置即可使用
