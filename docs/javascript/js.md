# javascript

## 循环

1.c 风格

    ```javascript
    // let 是 es6 引进的关键字, 用于声明块作用域的变量
    for (let i = 0; i < 10; i += 1) {
        console.log(i)
    }

    while (let i < 10) {
        console.log(i)
        i += 1
    }
    ```
2.数组循环

    ```javascript
    const length = myArray.length
    for (let index = 0; index < length; index += 1) {
        console.log(myArray[index])
    }

    myArray.forEach((value, index, arr) => {
        console.log(value)
    })
    ```

## 创建异步函数

### demo

```javascript
function demoAsync(callback) {
    setTimeout(callback, 5000, "simon", "male")
}

demoAsync((name, gender) => {
    console.log(name, gender)
})
console.log("code after running demoAsync")
```

### Promise

```javascript
const promise = new Promise((resolve, reject) => {
    setTimeout((name, gender) => {
        console.log(name, gender)
        resolve("code after running promise")
    }, 5000, "simon", "male")
})
promise.then(msg => console.log(msg))
```

## 变量提升/函数提升

用 var 声明的变量, 在声明前访问该变量只会得到 undefined 值, 而不会报错.
用 let/const/或者不加任何修饰 声明的变量, 在声明前访问都会报错.
所以, "先声明后使用"是条金科玉律

## 不理解的地方

### 未声明的变量执行除 typeof 之外的操作会报错. 未声明的属性却合法

```javascript
//Uncaught ReferenceError
console.log(noexist)

// undefined
const person = {}
console.log(person.noexist)
```

### Number.MAX_VALUE

```javascript
// true
Number.MAX_VALUE + 10000 === Number.MAX_VALUE
```

### 浮点数的最高精度是17位小数
```javascript
console.log(0.123456789012345678901234)
0.12345678901234568
```

### 为什么0.1 + 0.2 === 0.3 返回 false, 100.1 + 100.2 === 200.3 返回 true
