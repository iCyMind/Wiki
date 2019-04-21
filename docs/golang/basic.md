# golang

## 从 js 转过来遇到的坑
- 没有隐式的类型转化
- package 的 name 不需要双引号
- 字符串用双引号, 字符用单引号. 这点和c一样
- 没有while, for语句不用圆括号
- 所有的语句必须放在函数中, 而且必须要有main函数
- switch case 没有break. 可以没有condition, 此时相当于 switch true
- 数组的初始化用的也是{}, `var arr [3]int = [3]{1, 2, 3}`, 数组必须有长度, 没有长度的叫slice: `var sl []int = []int{1, 2, 3}`
    - 要创建动态长度的数组, `var arr [dx]int`这样是不行的. 要用make函数
    - 向数组添加元素, 仅仅`append(arr, 1)`是不行的, 要用`arr = append(arr, 1)`

