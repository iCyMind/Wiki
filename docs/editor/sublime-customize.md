# Sublime Text个性化

## 主题

- Material Theme. 试了很多UI，这个最美观
- Material Theme- Appbar. Material主题的插件，主要功能是定义⇥栏的颜色
- "theme": "Material-Theme.sublime-theme"
- "color_scheme": "Packages/Material Theme/schemes/Material-Theme.tmTheme", 这配色和Material Theme最配

## 被动插件

- all autocomplete

    Extends the default autocomplete to find matches in all open files. By default Sublime only considers words found in the current file. 自动补全当前文件中出现的词汇。 

- AutoFileName

    在编辑文件时，如果需要插入文件路径，这插件会补全文件名。

- BracketHighlighter

    高亮显示匹配的括号

- ConvertToUTF8/Codecs33

    让sublime支持utf-8之外的文件编码，打开gb2312之类的文件不再乱码。(但是不会帮你保存为utf8，除非通过"File-set File encoding to"菜单手动设置)

- SideBarEnhancements

    侧边栏增强，增加了很多右键菜单

- SublimeLinter

    代码提示（错误，警告等等）框架

- SublimeLinter-html-tidy

    This linter plugin for SublimeLinter provides an interface to tidy (either the html4 or html5 version). It will be used with files that have the “HTML” syntax. 依赖sublimeLinter插件，依赖系统的tidy命令。建议通过brew安装tidy-html5程序，以便新版本的tidy命令能为此插件所用，使得可以对html5代码进行检查

- SublimeLinter-ruby

    This linter plugin for SublimeLinter provides an interface to linting via ruby -wc. It will be used with files that have the “Ruby” syntax.

- SublimeLinter-jshint

    依赖nodejs和nodejs包（jshint）

- Color Highlighter

    ColorHighlighter is a plugin for the Sublime Text 2 and 3, which unobtrusively previews color values by underlaying the selected hex codes in different styles, coloring text or gutter icons. Also, plugin adds color picker, color format converter and less/sass/styl variables navigation to easily modify colors. 默认情况下，只是在以相同的颜色值下划线显示颜色。可在包的setting-user中将style/ha-style设置为filled（color text 更佳）。 有一个拾取颜色的命令: <kbd>⌃</kbd> <kbd>⇧</kbd> <kbd>C</kbd>

- LiveReload

    A web browser page reloading plugin for the Sublime Text 3 editor.安装完后，需要用<kbd>⌃</kbd> <kbd>⇧</kbd> <kbd>P</kbd>启用相应的插件（如simple reload）。然后安装livereload的浏览器插件，用浏览器打开本地文件后，点击liverealod的浏览器插件即可。

- MarkdownEditting

    高亮语法等等

## 主动插件

- acejump

    AceJump allows you to move the cursor to any character to any place currently on screen. To clarify, you can jump between characters in all visible portions of currently open documents in any panes. Like it's emacs counterpart, AceJump for sublime features word (on the image below), character and line modes which make jumping even easier. 快捷键(默认的快捷键被其他插件覆盖了，因此在user-keymap中重写了一遍）：

    - <kbd>⌘</kbd> <kbd>⇧</kbd> <kbd>;</kbd> = Jump to word
    - <kbd>⌘</kbd> <kbd>⇧</kbd> <kbd>'</kbd> = Jump to char
    - <kbd>⌘</kbd> <kbd>⇧</kbd> <kbd>.</kbd> = Jump to line

- advancednewfile

    Simply bring ↑ the AdvancedNewFile input through the appropriate key binding. Then, enter the path, along with the file name into the input field. ↑on pressing enter, the file will be created. In addition, if the directories specified do not yet exists, they will also be created. For more advanced usage of this plugin, be sure to look at Advanced Path Usage. By default, the path to the file being created will be filled shown in the status bar as you enter the path information.

    Default directory: The default directory is specified by the default_root setting. By default, it will be the top directory of the folders listed in the window. If this cannot be resolved, the home directory will be used. See Settings (default_root) for more information.

    - <kbd>⌘</kbd> <kbd>⌥</kbd> <kbd>N</kbd> = new file
    - <kbd>⌘</kbd> <kbd>⌥</kbd> <kbd>⇧</kbd> <kbd>N</kbd> = new dir

- Emmet

    web开发效率插件，快捷键相当多。

    - <kbd>⌃</kbd> <kbd>E</kbd> = Expand
    - <kbd>⌃</kbd> <kbd>A</kbd> ( <kbd>X</kbd> ) = +1(-1)
    - <kbd>⌥</kbd> <kbd>A</kbd> ( <kbd>X</kbd> ) = +0.1(-0.1)
    - <kbd>⌃</kbd> <kbd>⌥</kbd> <kbd>A</kbd> ( <kbd>X</kbd> ) = +10(-10)
    - <kbd>⌥</kbd> <kbd>M</kbd> = 跳到匹配的标签 ⌥+m（st自带跳到匹配的括号 <kbd>⌃</kbd> <kbd>M</kbd> ）
    - <kbd>⌃</kbd> <kbd>W</kbd> = wrap as you type
    - <kbd>⌃</kbd> <kbd>⌥</kbd> <kbd>→</kbd> = 下一个占位编辑点
    - <kbd>⌘</kbd> <kbd>⌃</kbd> <kbd>R</kbd> = 删除标签
    - <kbd>⌘</kbd> <kbd>⌥</kbd> <kbd>R</kbd> = 重构厂商前缀CSS属性的值
    - <kbd>⌘</kbd> <kbd>⇧</kbd> <kbd>R</kbd> = 重命名标签
    - `lorem`<kbd>⇥</kbd>, or `p*4>lorem` <kbd>⌃</kbd> <kbd>E</kbd> = 生成dummy text
    - 缩写：[emmet-cheat-sheet][emmet-cheat-sheet]

- SublimeCodeIntel

    Sublime​Code​Intel 是一个代码提示、补全插件，支持 JavaScript、Mason、XBL、XUL、RHTML、SCSS、Python、HTML、Ruby、Python3、XML、Sass、XSLT、Django、HTML5、Perl、CSS、Twig、Less、Smarty、Node.js、Tcl、TemplateToolkit 和 PHP 等语言，是 Sublime Text 自带代码提示功能的很好扩展。它还有一个功能就是跳转到变量、函数定义的地方，十分方便。

    - Jump to definition = <kbd>⌃</kbd> <kbd>CLICK</kbd>
    - Jump to definition = <kbd>⌃</kbd> <kbd>⌘</kbd> <kbd>⌥</kbd> <kbd>↑</kbd>
    - Go back = <kbd>⌃</kbd> <kbd>⌘</kbd> <kbd>⌥</kbd> <kbd>←</kbd>
    - Manual Code Intelligence = <kbd>⌃</kbd> <kbd>⇧</kbd> <kbd>␣</kbd>

- View In Browser

    Open the contents of your current view/⇥ in a web browser。默认快捷键 <kbd>⌃</kbd> <kbd>⌥</kbd> <kbd>V</kbd> 是用firefox打开。可以在`package setting/setting - user`中定义`browser`变量为你常用的浏览器。（在全局的`setting-user`中定义不起作用）

    - <kbd>⌃</kbd> <kbd>⌥</kbd> <kbd>F</kbd> - Firefox
    - <kbd>⌃</kbd> <kbd>⌥</kbd> <kbd>C</kbd> - Chrome
    - <kbd>⌃</kbd> <kbd>⌥</kbd> <kbd>S</kbd> - Chrome

- VintageousPluginSurround

    操作围绕单词的符号、标签。有文本：`"hello world!"`

    - <kbd>⌘</kbd> <kbd>⇧</kbd> <kbd>P</kbd>  = Command palette
    - <kbd>C</kbd> <kbd>S</kbd> <kbd>"</kbd> <kbd>'</kbd> => `'hello world!`
    - <kbd>C</kbd> <kbd>S</kbd> <kbd>'</kbd> `<q>` => `<q>hellow world!</q>`
    - <kbd>C</kbd> <kbd>S</kbd> <kbd>T</kbd> <kbd>"</kbd> => `"hello world!"`
    - <kbd>D</kbd> <kbd>S</kbd> <kbd>"</kbd> => `hellow world` 
    - <kbd>Y</kbd> <kbd>S</kbd> <kbd>I</kbd> <kbd>W</kbd> <kbd>]</kbd> => `[ hello ] world!`
    - <kbd>C</kbd> <kbd>S</kbd> <kbd>]</kbd> <kbd>}</kbd> => `{ hello } world!`
    - Finally, let's try out visual mode. Press a capital <kbd>V</kbd> (for linewise visual mode) followed by <kbd>S</kbd> `<p class="important">`.

## 半自动（不需要快捷键）

- FindKeyConflicts

    Assist in finding key conflicts between various plugins. This plugin will report back shortcut keys that are mapped to more than one package. This does not guarantee that the listed plugins are necessarily in conflict, as details, such as context, are ignored. This is simply a tool to help assist what plugins may be conflicting.

- Git

    集成git命令

- GitGutter

    A sublime text 2/3 plugin to show an icon in the gutter area indicating whether a line has been inserted, modified or deleted.

- Terminal

    Shortcuts and menu entries for opening a terminal at the current file, or the current root project folder in Sublime Text.

- HTML-CSS-JS Prettify

    HTML, CSS, JavaScript and JSON code formatter for Sublime Text 2 and 3 via node.js。依赖nodejs

- CSScomb

    对CSS属性进行排序，⌃+⇧+c

- Project Manager

    管理项目

- Gist

    管理Github的gist。我新建了一个Snippets的gist，把sublime的代码片段、有用的其他代码片段都上传到github上。同时chrome的gistbox扩展用来收集网上的代码片段。

## Sublime KeyMap

- <kbd>⌘</kbd> <kbd>⇧</kbd> <kbd>P</kbd>  = Command palette
- <kbd>⌘</kbd> <kbd>P</kbd> = Go to anything
- <kbd>⌘</kbd> <kbd>R</kbd> = Go to symbol
- <kbd>⌃</kbd>  <kbd>M</kbd> = Go to Match pairs
- <kbd>⌃</kbd>  <kbd>`</kbd> = Show console
- <kbd>⌘</kbd>  <kbd>⌃</kbd>  F Enter full screen
- <kbd>⌘</kbd>  <kbd>⌃</kbd> <kbd>⇧</kbd> <kbd>F</kbd> Enter distra­ction free mode
- <kbd>⌘</kbd> <kbd>/</kbd> = Toggle comment
- <kbd>⌘</kbd> <kbd>⌥</kbd> <kbd>/</kbd> = Toggle block comment
- <kbd>⌘</kbd> <kbd>D</kbd> = Expand selection to word
- <kbd>⌘</kbd> <kbd>L</kbd> = Expand selection to line
- <kbd>⌘</kbd> <kbd>⇧</kbd> <kbd>A</kbd> = Expand selection to tag (HTML/XML)
- <kbd>⌘</kbd> <kbd>⇧</kbd> = ␣ Expand selection to scope
- <kbd>⌃</kbd> <kbd>⇧</kbd> <kbd>M</kbd> = Expand selection to brackets
- <kbd>⌘</kbd> <kbd>⇧</kbd> <kbd>J</kbd> = Expand selection to indent­ation
- <kbd>⌘</kbd> <kbd>⌥</kbd> <kbd>G</kbd> = Quick find Searches for the word under the cursor 
- <kbd>⌘</kbd> <kbd>⌃</kbd> <kbd>G</kbd> = Quick find all Selects all occurences of the word under the cursor
- <kbd>⌘</kbd> <kbd>⌥</kbd>  <kbd>1</kbd> = Single
- <kbd>⌘</kbd> <kbd>⌥</kbd>  <kbd>2</kbd> = Columns: 2
- <kbd>⌘</kbd> <kbd>⇧</kbd> <kbd>⌥</kbd>  <kbd>2</kbd> = Rows: 2
- <kbd>⌘</kbd> <kbd>⇧</kbd> <kbd>⌥</kbd>  <kbd>3</kbd> = Rows: 3
- <kbd>⌃</kbd> <kbd>1</kbd> = moves to pane 1
- <kbd>⌃</kbd> <kbd>2</kbd> = moves to pane 2


[emmet-cheat-sheet]: http://docs.emmet.io/cheat-sheet/

