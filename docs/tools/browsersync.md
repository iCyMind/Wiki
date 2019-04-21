# Browsersync

## install

`npm install -g browser-sync`


## 静态文件

以下参数只监视根目录的文件

browser-sync start --server --files "*.*" --index index.html

## 同时监视多种类型文件

browser-sync start --server --files "**/*.html, **/*.css" --index index.html

[更多参数](https://www.browsersync.io/docs/command-line)
