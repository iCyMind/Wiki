# 图片懒加载

应该有几个关键点:
- 核心是一个图片标签是否已经进入 viewport, 包括从水平方向和垂直方向.
- 有良好的封装, 不可能在每一个 image 元素上都实现一遍
- 不仅要检测窗口的滚动, 还要检测 overflow 元素的滚动
- opacity 变成非0, visibility 变成非 hidden, 这些情况需不需考虑
- 懒加载不是重复加载, 已经加载过得元素不能再载.
- 图片未加载前的占位空间设置为多大
- 通过 css background 属性设置的图像也要考虑
- video 标签如何考虑

## 在合适的时机设置图片的 src 值

```js
// 将图片真实的地址放在 data-src 属性里
// 如果 src 为空, element.src 取到的值可能是 location.href, 要对这种情况进行排除
function setImageSrc (element, dataName) {
  if (element.src || element.src === location.href ) return
  if (element.dataset[dataName]) {
    element.src = element.dataset[dataName]
  }
}
```

## 检测 opacity/visibility 的变化

一般而言, MutaionObserver API 能检测一个节点的属性/子节点等等的变化. 但 opacity/visibility 的变化, 来源有:

1. 元素 style 属性里做更改
2. 元素的类名发生了改变, 在相应类名里做了属性的改变
3. 父元素的这些属性发生了变化

对于来源1, 可以观察元素的 style 值; 对于来源2, 可以观察元素的 class 值; 来源3比较复杂, 需要递归检测所以父级元素的变化. 懒加载是为了优化性能/用户体验而采用, 如果懒加载本身就给性能带来了影响, 那么并不值得做懒加载. 所以 opacity/visibility 的变化推荐采用手动的方法, 在执行变化的相关 js 代码里自行设置图片的 src 值.

## 元素是否已经进入视窗

```js

function lazyLoad () {
  let images = [...document.querySelectorAll('img.lazy')]

  function debounce (fn, wait) {
    let timerId
    let lastTriggerTime
    function timerExpired (...args) {
      const remainTime = wait - (Date.now() - lastTriggerTime)
      if (remainTime <= 0) {
        return fn.apply(this, args)
      }
      setTimeout(() => {
        timerExpired.apply(this, args)
      }, remainTime)
    }
    return function inner (...args) {
      if (timerId) return
      setTimeout(() => {
        timerExpired.apply(this, args)
      }, wait)
    }
  }
  function process () {
    images.forEach((img) => {
      if (img.style.display === 'none') return
      let { top, bottom, left, right } = img.getBoundingClientRect()
      if (top >=0 && top <= window.innerHeight && left >= 0 && left <= window.innerWidth) {
        img.src = img.dataset.src
        img.srcset = img.dataset.srcset
        img.classList.remove('lazy')
        images = images.filter(x => x !== img)
        if (images.length === 0) {
          window.removeEventListener('resize', debounceProcess)
          document.removeEventListener('scroll', debounceProcess)
          window.removeEventListener('orientationchange', debounceProcess)
        }
      }
    })
  }

  const debounceProcess = debounce(process, 200)

  window.addEventListener('resize', debounceProcess)
  document.addEventListener('scroll', debounceProcess)
  window.addEventListener('orientationchange', debounceProcess)

  // 手动执行一遍, 以加载首屏 lazy 资源( 但是其实首屏不应该懒加载 )
  debounceProcess()
}
```

这个实现, 有个 bug: 某个元素的坐标已经在视窗范围内, 在某种情形下它可能还是处于不可见的状态. 比如他的父级是可滚动的, 而又恰好没有滚动到它可见的位置. 如果要检测这种情况, 又需要递归地查询元素在父容器内是否可见了, 这会引起性能问题. 所以检测这种简单的情况就已经足够.

## 对背景图片进行懒加载

先约定, 需要懒加载背景的元素, 添加 lazy-background 类名, css 里为这个类添加占位图片, 而真实的背景图片设置在 .lazy-background.visible 里设置. 当元素进入视图时js为它添加一个 .visible 类名即可.

## 新的拯救者

如果不考虑 IE, 那么可以使用新的 IntersectionObserver API 来拯救.

```js
function modernLazyLoad () {
  let images = [...document.querySelectorAll('img.lazy')]

  const observer = new IntersectionObserver((entries, ob) => {
    entries.forEach((entry) => {
      if (!entry.isIntersecting) return
      entry.target.src = entry.target.dataset.src
      entry.target.srcset = entry.target.dataset.srcset
      entry.target.classList.remove('lazy')
      ob.unobserve(entry)
    })
  })
  images.forEach(img => observer.observe(img))
}
```

IntersectionObserver 可以处理元素在可滚动容器里嵌套的问题.


## 鱼与熊掌

```js

// 约定:
// 1. 资源的 src 地址放在 data-src 里
// 2. 资源的 srcset 地址放在 data-srcset 里
// 3. 需要懒加载的资源添加 lazy 类名

;(
  function () {

    function lazyLoad () {
      let images = [...document.querySelectorAll('img.lazy')]

      function debounce (fn, wait) {
        let timerId
        let lastTriggerTime
        function timerExpired (...args) {
          const remainTime = wait - (Date.now() - lastTriggerTime)
          if (remainTime <= 0) {
            return fn.apply(this, args)
          }
          setTimeout(() => {
            timerExpired.apply(this, args)
          }, remainTime)
        }
        return function inner (...args) {
          if (timerId) return
          setTimeout(() => {
            timerExpired.apply(this, args)
          }, wait)
        }
      }
      function process () {
        images.forEach((img) => {
          if (img.style.display === 'none') return
          let { top, bottom, left, right } = img.getBoundingClientRect()
          if (top >=0 && top <= window.innerHeight && left >= 0 && left <= window.innerWidth) {
            img.src = img.dataset.src
            // img.srcset = img.dataset.srcset
            img.classList.remove('lazy')
            images = images.filter(x => x !== img)
            if (images.length === 0) {
              window.removeEventListener('resize', debounceProcess)
              document.removeEventListener('scroll', debounceProcess)
              window.removeEventListener('orientationchange', debounceProcess)
            }
          }
        })
      }

      const debounceProcess = debounce(process, 200)

      window.addEventListener('resize', debounceProcess)
      document.addEventListener('scroll', debounceProcess)
      window.addEventListener('orientationchange', debounceProcess)

      // 手动执行一遍, 以加载首屏 lazy 资源( 但是其实首屏不应该懒加载 )
      debounceProcess()
    }

    function modernLazyLoad () {
      let images = [...document.querySelectorAll('img.lazy')]

      const observer = new IntersectionObserver((entries, ob) => {
        entries.forEach((entry) => {
          if (!entry.isIntersecting) return
          entry.target.src = entry.target.dataset.src || ""
          entry.target.srcset = entry.target.dataset.srcset || ""
          entry.target.classList.remove('lazy')
          ob.unobserve(entry.target)
        })
      })
      images.forEach(img => observer.observe(img))
    }

    document.addEventListener('DOMContentLoaded', () => {
      if ('IntersectionObserver' in window) {
        modernLazyLoad()
      } else {
        lazyLoad()
      }
    })
  }
)()
```

[Lazy Loading Images and Video](https://developers.google.com/web/fundamentals/performance/lazy-loading-guidance/images-and-video/)
