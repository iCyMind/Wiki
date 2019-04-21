# compress tools

- tar

    只负责打包/解包, 不负责压缩. 可以通过参数进行压缩, 或者通过管道传递给其他应用进行压缩.

    ```bash
    # 打包, c=create, v=verbose, f=file
    tar -cvf demo.tar ~/demo
    # 打包时排除某个目录
    tar --exclude="node_modules" -cvf demo.tar demo
    # 解包
    tar -xvf demo.tar
    # 解包到指定目录(前提条件是该目录存在)
    tar -xvf demo.tar -C ~/demo2

    # 打包同时压缩
    tar -czvf demo.tar ~/demo
    # 通过管道进行压缩
    tar -cvf - ~/demo | 7z a -si demo.tar.7z
    ```

- 7z

    压缩解压工具. 随便挑了一个node_modules文件夹来测试. 文件夹大小160M, tar.gz 压缩的结果是53M, 7z 压缩的结果是36M, 效率要高30%.

    ```bash
    # 压缩
    7z a demo.7z ~/demo
    # 压缩, 排除某个文件夹
    7z a demo.7z ~/demo -x'!node_modules'
    # 解压
    7z x demo.7z
    # 解压到指定文件夹, 不能用~等简写. -o 后面不能接空格
    7z x demo.7z -o/Users/simon/demo
    ```
