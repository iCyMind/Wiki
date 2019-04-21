# wiki engine

## 需求
对 wiki 引擎, 有几个需求:
1. markdown 语法. markdown 是通用的, 而且满足我的表达需求. 而各种 wiki 语法都有额外的学习成本.
2. 能本地编辑. 这样就可以用自己喜欢的编辑器来编辑, 而不是被绑定在网页上.
3. 纯文本. 方便迁移, 同步.
4. 简洁美观. 不好看宁勿死.

## 候选

### gollum
目前在用. github/gitlab 所用的 wiki 引擎. 纯文本, 支持 markdown 以及其他的语法. 本地编辑 git 提交生效.

### mkwiki
有个严重的 bug: 当 code fence 用了不支持的语言时, 会进入死循环而卡死, 慎用.

### simiki
类似 hexo , 还需要生成静态页面.


## gollum 的使用
之前是通过 docker 使用 gollum 的, 但是 docker 消耗资源实在太大. 于是切换成非 docker 方案

- 安装: 通过 rbenv 安装 ruby, 然后 gem install gollum
- 编写启动脚本:

  ```bash
  #!/bin/bash

  DIR="$( cd -P "$( dirname "${BASH_SOURCE[0]}" )" && pwd )"
  eval "$(/usr/local/bin/rbenv init -)"
  gollum --port 6060 --no-edit --h1-title --no-live-preview --js --config $DIR/../config/config.rb --ref dev $DIR/../
  ```
- 开机自动启动

  ```
  <?xml version="1.0" encoding="UTF-8"?>
  <!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
  <plist version="1.0">
  <dict>
    <key>Label</key>
    <string>com.icymind.wiki</string>
    <key>ProgramArguments</key>
    <array>
      <string>sh</string>
      <string>-c</string>
      <string>/Users/simon/Dropbox/PKM/config/gollum.sh</string>
    </array>
    <key>RunAtLoad</key>
    <true/>
    <key>KeepAlive</key>
    <true/>
  </dict>
  </plist>
  ```
- 开机启动的坑

因为 gollum 是通过 rbenv 安装的, 所有在执行之前一定要做 rbenv 的初始化. 而这些初始化的设置只能影响到当前的 shell, 所以对 launchd 不起作用. 解决方案是通过 `sh -c script-path` 来启动一个 shell, 然后在脚本里做相应的初始化操作
