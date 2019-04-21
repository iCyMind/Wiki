# OSX

### 查看端口占用
`netstat -an | grep 5555` or `lsof -i:5555`

### brew
- brew leaves
- brew search sth
- brew cask search sth
- brew bundle dump
- brew home vim: 打开 vim 的主页

## launchctl 后台服务

更多内容: `man launchctl`

后台服务有两类: 守护(Daemons)和代理(Agents)

- Daemons 运行在全局语境, 无法和用户直接交互(只能被动响应). 可执行文件的权限为: root:wheel
- Agents 运行在用户语境, 可以和用户交互. 用户的 Agents 权限为用户及其组, 全局 Agent 的权限是 root:whell
- 

相关路径：

- ~/Library/LaunchAgents: 用户登录后才开启相关服务, owner 为 simon:staff
- /Library/LaunchDaemons: 系统启动时就开启相关服务, owner  为 root:wheel

launchctl 2.0 进行了一些改写，很多教程里的命令都是旧的。新版本有 target 的概念，对我来说常用的有两种：

1. system/com.icymind.ss
2. gui/501/com.icymind.ss: 501 是用户的 id ，通过 `id -u simon` 得到

注意事项
- 执行的命令需要用完全路径(包括脚本中命令的路径), 否则无法启动
- UserName 可以在 System Daemon 中指定用户. 在 User Agent 中无法使用.
- 如果要 keepalive , 那么运行的脚本要写成持续运行的形式. 如果脚本运行完就马上退出, 那么 launchd 服务会一直重新运行重新运行. 脚本范例:

    ```bash
    #!/bin/bash
    fucntion stop()
    {
        echo "$(date): stop function being called."
        exit 0
    }
    function start()
    {
        echo "$(date): start function being called."
        # 后台运行tail -f /dev/null, 该命令除非收到 TERM KILL 等信号, 不然不会退出. 类似的功能有 read/cat 等等
        tail -f /dev/null &
        # 等待 tail 进程执行完毕
        wait $!
    }
    trap stop HUP KILL TERM
    start;
    ```

常用命令(注意 plist 中不要使用 $HOME 等变量，不然无法启动该服务。还要注意 后面跟的是文件路径，这跟其他命令的格式有点区别):

- `launchctl bootstrap gui/501 $HOME/com.icymind.ss.plist`: 加载相关服务并运行，相当于以前的 load, 注意填写 plist 的路径
- `launchctl bootout gui/501/com.icymind.ss`: 卸载相关服务，相当于以前的 unload, 不需要写 plist 路径
- `sudo launchctl bootstrap/bootout system com.icymind.ss.plist`

- `launchctl kickstart -kp gui/501/com.icymind.ss`: 可以用于重启服务
- `sudo launchctl kickstart system/com.icymind.ss`

- `launchctl print gui/501`: 相当于以前的 list
- `sudo launchctl print system`: 相当于以前的 list

- `launchctl kill SIGTERM/SIGKILL gui/501/com.icymind.ss`
- `plutil -lint com.icymind.ss.plist`: 检查 plist 文件语法


## network

### config network with command line
- `networksetup -setdnsservers Wi-Fi 192.168.250.1 `
- `sudo route change default 192.168.250.1`
