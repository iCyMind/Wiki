# Vue

## 相关资源

- 单元测试工具 [vue-test-utils](https://vue-test-utils.vuejs.org/zh-cn/)
- 代码风格 [vue style](https://cn.vuejs.org/v2/style-guide/index.html)
- 项目配置 [vue-loader](https://vue-loader.vuejs.org/zh-cn/)

## 测试

- [vue-test](./vue-test)

## 踩过的坑

- computed 属性必须被其他地方引用, 才会触发计算. 否则即使计算 computed 属性里的依赖已经更新, 也不会发生任何事. 如果实在没有被引用, 比如

```javascript
<script>
  props: ['objFromParent'],
  data () {
    return {
      obj: {}
    }
  },
  computed: {
    copyOfObjFromParent () {
      const copy = JSON.parse(JSON.stringify(this.objFromParent))
      this.obj = copy
      return copy
    }
  }
</script>
```

  以上例子, 计算属性只是用来做一个临时的转化过程, 把 props 的值拷贝为组件内部的值(之所以没用 obj 作为一个计算属性是因为组件内部可能会对 obj 进行写入操作.)

  以上例子如果 view 中没有引用 copyOfObjFromParent 的话, copyOfObjFromParent 永远不会得到更新, 结果就是 this.obj 也不会得到更新. 所以这种情景下用 watch 属性比较合适.

```javascript
<script>
  props: ['objFromParent'],
  data () {
    return {
      obj: {}
    }
  },
  watch: {
    objFromParent: {
      handler: function (val, oldVal) {
        this.obj = JSON.parse(JSON.stringify(val))
      },
      deep: true
    }
  }
</script>
```
而且 watch 还要带 deep 开关, vue 才会监视组件的属性的变化, 否则只有在属性被完全替代(即赋值)时才会触发 handler.

- 永远不要在 computed 属性的 setter 里改变该计算属性的值, 否则会死循环.
- 组件声明周期的顺序: 父组件created/子组件created/子组件mounted/父组件mounted
- 生命周期之间的顺序无法利用 async/await 来保证. 按理来说, created 的钩子比 mounted 先执行, 但是如果这么写:

```javascript
async created () {
    await new Promise((resolve, reject) => {
        setTimeout(() => {
            console.log('created')
            resolve()
        }, 1000)
    })
},
mounted () {
    console.log('mounted')
}
```
  这样写并不能保证 'created' 在 'mounted' 之前打印. 这是 vuejs 体质决定的. [issue](https://github.com/vuejs/vue/issues/7209)
