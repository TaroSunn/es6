# let命令
### 基本用法

用法类似于var,用于声明变量,但是所生命的变量只在let命令所在的代码块内有效
```
{
  let a = 10;
  var b = 1
}
console.log(a) // 报错 a is not defined
console.log(b) // 1
```
let 声明的变量只有在其代码块内有效

```js
var a = []
for(let i = 0;i<10;i++){
  a[i] = function(){
    console.log(i)
  }
}
a[6]() // 6
```
for 循环中，设置循环变量的那部分是一个副作用域，而循环体内部是一个单独的子作用域

``` js
for(let i = 0;i<3;i++) {
  let i = 'abc'
  console.log(i)
}
// abc
// abc
// abc
```

### 不存在变量提升
var 命令会发生'变量提升'现象，即变量可以在声明之前使用，值为undefinded。按照一般的逻辑，变量应该在声明语句之后才可以使用，

let 命令改变了语法行为，他所声明的变量一定要在声明后使用，否则会报错

```js
// var 情况
console.log(foo) // undefined
var foo = 2

// let 情况
console.log(bar) // 报错
let bar = 2
```

## 暂时性死区
只要块级作用域内存在let命令，它所声明的变量就'绑定'这个区域，不再受外部的影响

``` js
var tmp = 123
if(true) {
  tmp = 'abc'
  let tmp
}
```
上面代码中存在全局变量tmp,但是块级作用域内let又声明了一个局部变量tmp,导致后者绑定了这个块级作用域,所以在let声明变量前，对tmp赋值会报错

ES6规定,如果区块中存在let和const命令,则这个区块对这些命令声明的变量从一开始就形成封闭作用域,只要声明之前就使用这些变量就会报错

总之，在代码块内，使用let命令声明变量之前，该变量都是不可用的，称为'暂时性死区'(简称TDZ)
```js
if(true) {
  // TDZ 开始
  tmp = 'abc' // 报错
  console.log(tmp) // 报错
  let tmp // TDZ结束
  console.log(tmp) // undefined
  tmp = 123
  console.log(tmp) // 123
}
```

所以变量一定要在声明之后使用,否则就会报错

### 不允许重复声明
let 不允许在相同作用域内重复声明一个变量

## 块级作用域

允许在块级作用域内声明函数

# const 命令

### 基本用法
const 声明一个只读的常量,一旦声明常量的值就不能改变

const 声明的常量不得改变值，意味着，const一旦声明常量,就必须立即初始化，不能留到以后赋值
```js
const foo // 报错
```
const 只在声明所在的块级作用域内有效

const 声明的常量不会提升，同样存在暂时性死区，只能声明之后使用，并且不可重复声明

const 实际上保证的不是变量的值不得改动,而是变量指向的那个内存地址不得改动，对于基本数据类型来说,值就是保存在变量指向的内存地址中，因此等同于常量。但对于复合类型的数据而言，变量指向的内存地址保存的是一个指针,const只能保证这个指针是固定的