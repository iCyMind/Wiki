# Markdown

## header

标题和中文之间留一个空行, 兼容性最好.

## list

list 块的前后都留一个空行, 兼容性最好.

list 条目的标题和它的内容之间留一个空格, 而且内容缩进 4 个空格:

<pre>
1. First step

    Demo of first step.

    Sub list of first step:

    - list 1.0
    - list 1.1

    Continue first step.

2. Second step
3. Last step
</pre>


以上写法将得到:

1. First step

    Demo of first step.

    Sub list of first step:

    - list 1.0
    
        sub step of first step.

    - list 1.1

    Continue first step.

2. Second step
3. Last step

Chrome 插件 MarkView 的实现有个 bug , 它会把 "Continue first step" 变成 二级 list.

gollum/ghost 对这个的实现和预期效果一样

## 代码块和 list

在 list 中嵌入代码块时, 代码块的缩进要和当前 list 条目的缩进一样. 一级 list 项目的代码块, 要缩进 4 个空格, 2 级的缩进 8 个. 而且代码块前后要留一个空行. 如:

<pre>
1. First step

    Demo of first step.

    Sub list of first step:

    - list 1.0

        ```bash
        #!/bin/bash

        echo "demo"
        echo "code block of sublist"

        exit 0
        ```

    - list 1.1

    Continue first step.

2. Second step

    ```bash
    #!/bin/bash

    echo "demo"
    echo "code block of Second step"

    exit 0
    ```

3. Last step
</pre>

得到的效果是:

1. First step

    Demo of first step.

    Sub list of first step:

    - list 1.0

        ```bash
        #!/bin/bash

        echo "demo"
        echo "code block of sublist"

        exit 0
        ```

    - list 1.1

    Continue first step.

2. Second step

    ```bash
    #!/bin/bash

    echo "demo"
    echo "code block of Second step"

    exit 0
    ```

3. Last step

MarkView 的效果可以保证代码块和它对应的 list 条目处于同一缩进水平, 但是代码行的内容却多了一层缩进.

ghost 也可以保证代码块缩进水平的正确, 但是代码块变成了单行代码, 效果很差. 目前官方还没有修复这个 bug , 只能把所有代码块顶格写.

gollum/github 的效果最好.

意外的发现是除了 github/ghost , 所有的对 pre 标签内嵌入代码块的处理都不行.
前后留空格的话, 在各个 parser 中都有一致的表现, 兼容性最好.
