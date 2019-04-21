# Webpack

## Plugins

### copy-webpack-plugins

利用这个插件， 可以将指定路径的文件拷贝到 output 路径。比如在 src/static 文件夹内保存有 index.html，
那么可以做如下配置：

```json
plugins: [
    new CopyWebpackPlugin([{
        from: ‘src/static’
    }])
```

执行 webpack 命令后， index.html 会出现在 output 路径下。


## Loaders

## Dev

### webpack-dev-server

webpack-dev-server 在 webpack.config.js 内通过最顶层的属性 devServer 进行配置，该属性是个对象。
```json
devServer: {
    contentBase: './dist'
}
```

webpack-dev-server 启用后，如果对依赖进行了改动， bundle 之类的输出文件不会再 output 路径下生成， 他们只存在于内存内。
devServer 可以通过以下属性进行配置：

- contentBase

    最好用绝对路径， 表示服务器的根目录. 文档里说如果不指定，将采用当前路径作为默认值。但是试过， 发现服务器仍然能找到output路径内的文件
