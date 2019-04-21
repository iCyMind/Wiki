# ripgrep

## 搜索文件名

- `rg -g "*conflicted*" --files`
- `rg -g "*conflicted*" --files -0 | xargs -0 rm`

## 搜索文件

- `rg top-singer -l | xargs sed -i '' 's/top-singer/your-project-name/g'`

