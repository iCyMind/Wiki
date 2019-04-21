# Shell

## bash命令
- sort如何：忽略起始的空白/忽略大小写/按月份排序/反序排序/如何按照uid对passwd 排序

    - -b/-f/-M/-r
    - `cat /etc/passwd | sort -t ':' -k 3 -n`

- grep如何输出不匹配模式的行？如何显示行号？如何匹配正则表达式

    - `-v/-n`
    - `grep [ab] filename`

- 如何向进程发出 TERM/INT/HUP/KILL 信号，各信号之间有什么区别

    - `kill -s TERM pid`
    - TERM是kill 命令默认发送的信号，最温柔。KILL 最无情。

- 如何用 tar 创建/显示/提取归档。.tgz是什么文件，如何获得内容？

    - `tar -cvf name.tar file1 file2`
    - `tar -xvf name.tar`
    - tgz是用 gzip 压缩过的归档文件，用tar -zxvf 提取

- umask值是怎么工作的？

    - 比如默认的 umask 是0022，那么由于默认的文件权限是666，默认的文件夹权限是777，那么新建的文件/文件夹权限则是644(666-022)/755

- 如何使得shared_Folder文件夹下所有的新建文件都属于shared组？

    - 首先将shared_Folder文件夹的属组改为 shared：chgrp shared shared_Folder
    - 其次设置 s 权限(运行时设置 SID 或者 GID): chown g+s shared_Folder
    - 为了能起到分享的作用，每个 shared 组的成员都将 mask 设置为对属组可写：umask 002

## 基本脚本
- 赋值语句，=两边是否需要空格

    - 不能有空格

- 什么时候使用变量需要添加$，什么时候不需要

    - 赋值的时候不能加$, 引用的时候需要

- 如何使用内联重定向

    - 用<< 以及自己指定的分隔符
    - EOF

        ```bash
        echo << EOF > save_file  
        first line  
        last line  
        EOF
        ```

- expr 的更好的替代品是什么，为什么

    - $[]更好，因为方括号里的操作符不用转义

- $[]方括号里需要加空格吗, 为什么加空格更好

    - 虽然不用加空格，但是加空格更一致，因为test 语句的[]需要加空格

- sort 命令不是按照 ascii 码排序的，而是使用了系统的本地化语言设置中定义的排序顺序

## if-then
- 强烈推荐用双中括号代替单[], [] 和 test 是同等的 builtin 命令, 而双中括号是 Shell 的关键字.

    - 双中括号用 && || 表示逻辑, 而 [] 中用 -a -o 表示逻辑
    - 双中括号比较字符串可以用双等号, [] 中用双等号达不到目的

- if 测试的语句的 true 和 false？

    - 不是，if 是个根据语句返回值是否为0来决定运行的分支, 所以:
    - 测试其他命令的执行情况:

        ```bash
        if running_cmd &> /dev/null;then
            echo "excute sucess"
        fi

        # or:
        if ! running_cmd &> /dev/null;then
            echo "excute failed"
        fi

        # 或者更简单
        pgrep kcptun && echo "running"
        ```


- test语句可以用[]代替，需要在哪里加空格？

    - 方括号里侧需要添加空格

- 字符串的相等测试的符号是=还是==?

    - 单括号里用=, 双括号里用==

- 字符串测试的>以及<有什么坑？

    - [ $str1 > $str2 ], 比较符需要转义为`\>`

- [ -z $var1 ]，如果 var1从未定义，测试结果如何？

    - 未定义的变量长度也是0

- 如何测试一个文件存在并非空？

    - `[[ -s file ]]`

- 如何比较字符串和正则表达式？

    - `[[ $str1 == log* ]]`

- 如何使用 case 语句简化 if-then

    ```bash
    case $var1 in  
        openwrt) cmd1;;  
        aliyun) cmd2;;  
        *)  cmd3;;  
    esac
    ```
- 测试字符串: `[[ -n $maynot ]] && echo "not zero"`, `[[ -z $mayzero ]] && echo "zero"`

## loop

- 如何向已有的列表添加新项

    - list="aa bb cc"
    - list=$list" dd"

- 什么是 IFS ，IFS=$'\n:;"'表示什么意思，为什么要在最开头添加$符号

    - Inter Field Separator, IFS=$'\n:;"'表示换行符/:/;/"这几个符号都被拿来做分隔符，在 for $item in $list中，$list变量会被这几个符号分割

- 如何使用 C 风格的 for 循环

    - for (( a = 0; a < 10; a++ )), 给变量赋值等号两侧可以加空格，引用变量不用加$符号，迭代计算也不需要 expr。
    - zsh 里的循环是这样的: `for i in 1 2 3;echo $i`, 也可以用`seq 1 10`生成序列

- while 的多个测试命令是怎么样的

    - while cmd1
        cmd2; do
    - 只有最后一个测试命令的退出码会被用来决定什么时候推出循环。但是每次迭代所有的测试命令都会被执行，包括最后一次。
    - 每个测试命令都在单独的一行。

- shell 的 break 和 continue 和高级语言相比有何不同

    - shell 的可以带参数，C 语言的不可以。

## 处理用户输入
- 第十个参数该怎么引用？

    - ${10}

- 如何读取程序本身的名称(不包含路径)

    - $(base $0)

- 如何获得参数数量？

    - $#

- 如何获得最后一个参数？

    - ${!#}。${$#}会出错

- 如何获得所有的参数数据？不同的方法之间有何异同

    - $* 和 $@
    - for $item in "$*" 中，只能得到一个整体的参数。如果换成"$@"，可以得到一个个单独的参数。

- 如何用 shift 循环获得参数值

    - 读取$1, shift, 再读取$1

- 如何用 getopt 分离选项和参数，getopt 有什么短板

    - getopt optstring options parameters
    - getopt -q ab:cd -a -b test1 -cde test2 test3, 得到：-a -b 'test1' -c -d -- 'test2' 'test3'
    - 短板是：如果一个参数含有空格，那么会被识别成两个参数
    - -q可以去掉错误消息，例子中-e 就是未知的选项

- getopts 又该如何使用，它有哪些关键变量

    - while getopts :ab:c opt; do  
        case $opt in ....
    - $OPTARG/OPTIND
    - optstring 前加一个冒号的作用和getopt 的-q 一样

- echo如何去掉末尾的换行符

    - echo -n "some"

- 如何优雅地读取用户一个或者多个输入并赋值给变量, $REPLY变量表示什么意思

    - read -p "Your name: " var1 var2
    - 如果没有提供变量，那么用户的输入会被保存到$REPLY变量中

- 如何设定用户输入的时限

    - read -t 5

- 如何在输入敏感数据时隐藏输入

    - read -s -p "Your password:" pwd

- 如何实现"按任意键继续"的功能

    - read -n1 -p "Press any key to continue..."

- 如何读取文件并对行进行处理

    - cat file | while read line
    - 或者是先设置 IFS，然后for line in $(cat file), 最后再恢复 IFS

## 呈现数据

- 如何重定向输入/输出/错误到文件？用时重定向和错误又该怎么做？

    - 重定向输入: cat < file
    - 重定向输出: echo "hello" > file
    - 重定向错误: cat noexits.txt 2> file
    - 同时重定向: cat maybeexits.txt &> file, cat maybeexits.txt >file 2>file

- 如何永久重定向？
- 如何创建临时文件/目录

    - mktemp / mktemp -d

- 如何同时在屏幕和文件中记录消息？如何追加消息？

    - echo "some" | tee file
    - echo "some" | tee -a file

## 数组

- 如何创建数组，访问数组，获得数组的长度

    - array_name(value0 value1 value2)
    - ${array_name[0]}。访问所有元素：${array_name[@]}
    - ${#array_name[@]}

## 一些工具

- 获取 ip

    - 国内 `curl -sS ip.3322.net`
    - 国外 `curl -sS icanhazip.com`
    - s 表示 silent, S 表示 show error

