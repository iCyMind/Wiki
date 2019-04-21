# 函数去抖动以及函数节流

js 高级编程里认为 throttle 是去抖动, 但很多第三方库里去抖动是 debounce, 节流是 throttle. 两者都是控制函数执行速率的一种方法, 含义略有不同, 这里采用后者的说法:

- debounce 函数去抖动. 如果一个事件不停地被触发, 回调事件仅在事件触发 n 秒后才执行. 如果未到时间又触发了一遍, 那么前一次的回调取消. 比如 resize 窗口
- throttle 函数节流, 如果一个事件不停地被触发, 那么在一段时间内, 回调函数最多执行一次.


## 简单的 debounce 实现

简单写一个debounce 的实现:
```js
function debounce (fn, wait) {
  let timerId
  return function inner (...args) {
    clearTimeout(timerId)
    setTimeout((args) => {
      fn.apply(this, args)
    }, wait)
  }
}
```
但是这个实现每次都要 clearTimeout/setTimeout, 其实是很没有必要的. 如果这个事件高频地触发, 那么性能可能会很差. 利用这个去抖动测试一个简单的 console.log 函数, 不断地触发 1e5 次, 耗时 1000 ms, 而用 lodash 的实现, 只需要 25 ms.

## 仅设置一个定时器

其实可以只设置一个定时器, 定时器到期的时候再检查, 如果满足执行条件就执行, 否则在续命 n 秒. n 应该最近一次触发时间到写在的时间值和 wait 的差值, 即 wait - (Date.now() - lastEmitTime).

```js
function debounce (fn, wait = 0) {
  let lastEmitTime
  let lastInvokeTime
  let timerId
  let timerExpired = function (args) {
    const remainTime = wait - (Date.now() - lastEmitTime)
    if (remainTime <= 0) {
      timerId = null
      return fn.call(this, args)
    }

    timerId = setTimeout(() => timerExpired.apply(this, args), remainTime)
  }
  return function inner (...args) {
    lastEmitTime = Date.now()
    if (!timerId) {
      timerId = setTimeout(() => timerExpired.apply(this, args), wait)
    }
  }
}
```

