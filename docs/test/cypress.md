# Cypress.io

## 安装
非 vue 项目可以直接看官网文档. 如果是 vue-cli 3 创建的项目, 使用 `vue add @vue/e2e-cypress` 命令安装到项目, [文档](https://github.com/vuejs/vue-cli/tree/dev/packages/%40vue/cli-plugin-e2e-cypress).

## 使用
vue 项目, cli 工具会自动创建一个 cypress.json 文件, 利用 pluginsFiles 的选项, 告诉 cypress 去哪里找测试文件, 又将截图/视频等保存到何处:
```json
// options: https://docs.cypress.io/guides/references/configuration.html#Options
{
  "pluginsFile": "tests/e2e/plugins/index.js"
}
```

```javascript
// tests/e2e/plugins/index.js

// https://docs.cypress.io/guides/guides/plugins-guide.html
module.exports = (on, config) => (
  Object.assign({}, config, {
    fixturesFolder: 'tests/e2e/fixtures',
    integrationFolder: 'tests/e2e/specs',
    screenshotsFolder: 'tests/e2e/screenshots',
    videosFolder: 'tests/e2e/videos',
    supportFile: 'tests/e2e/support/index.js'
  })
)
```

使用 `npm run test:e2e` 执行示例测试, cypress 会以 production 模式构建代码, 然后以 dev-server 运行. 如果不想每次执行测试都跑一遍构建的过程, 可以加入 --url 参数, 让 cypress 连接 url 进行测试: `npm run test:e2e -- --url http://localhost:2019`

## 编写测试

- 元素的互动
  在进行元素的互动前, cypress 会进行若干的检查和操作, 包括:
  - 滚动元素到视窗内
  - 确保元素可见
  - 确保元素非禁用
  - 确保元素不处于动画状态
  - 确保元素不被遮盖
  - 如果被 fixed 的元素遮挡, 滚动页面
  - 在期望的位置发起相应事件

  以上任何步骤失败都会导致和元素的互动失败

- [Visibility](https://docs.cypress.io/guides/core-concepts/interacting-with-elements.html#Visibility)
  除了常见的元素, 需要注意 overflow: hidden 的情况.

- Disability
  只检查 disabled, 不检查 readonly
- Animations
  高阶操作, 暂时用不上
- Covering
  检查一个元素是否被覆盖, 只检查中点
- 滚动
  滚动操作只发生在和元素的互动上, 如果执行的只是 cy.get/cy.find, 是不会执行滚动的.
- 坐标
  时间通常在元素的中点触发, 但是也可以指定位置: `cy.get('buttton').click({ position: 'topLeft' })`

