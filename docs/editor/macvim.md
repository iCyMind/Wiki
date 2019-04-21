# MacVim

## MacVim 无法占满屏幕高度

https://github.com/macvim-dev/macvim/issues/78

```bash
# macbook 13", font size: 14px
defaults write org.vim.MacVim MMTextInsetTop 22
defaults write org.vim.MacVim MMTextInsetBottom 14
defaults write org.vim.MacVim MMTextInsetLeft 14
defaults write org.vim.MacVim MMTextInsetRight 26
```
