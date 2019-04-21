# Mackup

用于管理 dotfiles, 方便备份和恢复.

## 备份
执行 mackup backup 即可. 它预设了很多配置文件的位置, 会自动将 dotfiles 拷贝到指定的位置(默认是 dropbox), 然后做文件链接到 $HOME 中默认的位置.

每次有新的文件需要备份, 都需要执行一次 `mackup backup`. 备份后每次编辑, 编辑的都是链接文件.

## 恢复
restore 即可.

## 自定义

有些应用的配置文件 mackup 并不支持, 不过没关系, 自定义自己的应用很方便.
在 $HOME 下新建文件夹 .mackup, 在文件夹内为每一个自定义的应用建立一个 cfg 文件

### 在 $HOME/.mackup.cfg 中设定忽略某个应用的设置

```
[storage]
engine = dropbox

[applications_to_ignore]
ssh
spacemacs
emacs
neovim
```

### fzf

```
[application]
name = fzf

[configuration_files]
.fzf.bash
.fzf.zsh
.fzf.sh
```

## neovim

```
[application]
name = custom-neovim

[configuration_files]
.config/nvim/init.vim
```
