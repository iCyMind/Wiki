# vim

## Plugin

### vimwiki

- 如果开启了 tablemapping, 那么会改写会绑定 tab 键. 如果同时使用 ultisnips , 那么 tab 就会变得无效.
- 将 sw 改为2, 可以使 Gollum 正确显示日志列表. 按道理 markdown list 的缩进是4 个空格. 虽然2个空格 Gollum 也能正常显示. 但也不是办法不是. 于是提交了一个 PR, 当当前语法为 markdown 时, 不缩进日志列表

```
= My knowledge base =
    * Tasks -- things to be done _yesterday_!!!
    * Project Gutenberg -- good books are power.
    * Scratchpad -- various temporary stuff.
```
Place your cursor on Tasks and press Enter to create a link. Once pressed, Tasks will become [[Tasks]] -- a Vimwiki link. Press Enter again to open it. Edit the file, save it, and then press Backspace to jump back to your index.

normal mode:

```
<Leader>ww -- Open default wiki index file.
<Leader>wt -- Open default wiki index file in a new tab.
<Leader>ws -- Select and open wiki index file.
<Leader>wd -- Delete wiki file you are in.
<Leader>wr -- Rename wiki file you are in.
<Enter> -- Follow/Create wiki link
<Shift-Enter> -- Split and follow/create wiki link
<Ctrl-Enter> -- Vertical split and follow/create wiki link
<Backspace> -- Go back to parent(previous) wiki link
<Tab> -- Find next wiki link
<Shift-Tab> -- Find previous wiki link
For more keys, see :h vimwiki-mappings
```

### How to see which plugins are making Vim slow?


```vim
:profile start profile.log
:profile func *
:profile file *
" At this point do slow actions
:profile pause
:noautocmd qall!
```

[How to see which plugins are making Vim slow?](https://stackoverflow.com/questions/12213597/how-to-see-which-plugins-are-making-vim-slow)

### Profiling Vim startup time
`vim --startuptime vim.log`


### Read/Save file as encoding
```
// read:
:e ++enc=utf-8

// save
:wq ++enc=utf-32
```
