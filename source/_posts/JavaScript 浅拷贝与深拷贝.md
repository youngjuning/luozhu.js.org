---
title: JavaScript 浅拷贝与深拷贝
description: 本文介绍了 JavaScript 中的浅拷贝与深拷贝
date: 2020-02-07 12:00:57
categories:
  - [前端, JavaScript]
tags:
  - JavaScript
  - 浅拷贝
  - 深拷贝
  - 基础类型
  - 引用类型
---

## 基础类型值和引用类型值的区别

### 保存方式

> 基础类型：变量 -> 值
> 引用类型：变量 -> 值的堆内存地址 -> 值

变量保存在栈内存中，栈内存是有序排列的，大小是固定的。在将一个值赋值给变量时，解析器必须确定这个值是基本类型值还是引用类型值。也就是说变量的值到底存的是实际值还是实际值的引用是根据实际值的类型决定的：基础类型的值是保存在变量中实际值，而引用类型的值是保存在堆内存中的堆内存是无序排列的，大小是不固定的），变量中存的值是实际值的堆内存地址。

### 复制变量值

> 基础类型：变量a -> 值；变量b ->值的副本
> 引用类型：变量b -> 变量a -> 值的堆内存地址 -> 值

在 ECMAScript 中，除了保存的方式不同之外，在从一个变量向另一个变量复制基本类型值和引用类型值时，也存在不同。

如果从一个变量向另一个变量复制基本类型的值，会在变量对象上创建一个新值，然后把该值复制到为新变量分配的位置上。新值只是一个副本。

当从一个变量向另一个变量复制引用类型的值时，同样也会将存储在变量对象上的值复制一份放到为新变量分配的空间上。不同的是，这个值的副本实际上是一个指针，而这个指针指向存储在堆中的一个对象。复制操作结束后，两个变量实际上将引用同一个对象。因此改变其中一个变量，就会影响另一个变量。

### 传递参数

ECMAScript 中所有函数的参数都是按值（变量的值，不一定是实际值）传递的。也就是说，把函数外部的值复制给函数内部参数，就和把值从一个变量复制给另一个变量是一样的。

在向参数传递基本类型的值时，被传递的值会被复制给一个局部变量（即命名参数，或者用 ECMAScript 的概念来说，就是 `arguments` 对象中的一个元素）。

在向参数传递引用类型的值时，会把这个值在内存中的地址复制给一个局部变量，因此这个局部变量的变化会反映在函数的外部。虽然变量是按值传递的，但是这个值它是实际值的引用，所以局部变量也是按照这个引用访问了同一个对象。

## 浅拷贝

浅复制只复制一层对象的属性，也就是说拷贝原对象的实例，但是对其内部的引用类型值，拷贝的是其引用

### 自行实现浅拷贝

```js
function shallowCopy(src) {
  var dst = {}
  for (var prop in src) {
    if (src.hasOwnProperty(prop)) {
      dst[prop] = src[prop]
    }
  }
  return dst
}
var obj = { a:1, arr: [2,3] }
var shallowObj = shallowCopy(obj)
// shallowObj.a = 1
// console.log(obj) // { a:1, arr: [2,3] }

// shallowObj.arr[0] = 1
// console.log(obj) // { a:1, arr: [1,3] }

// shallowObj.arr = [1,3]
// console.log(obj) // { a:1, arr: [2,3] }
```

### 工程化

- [_.clone](https://www.lodashjs.com/docs/latest#_clonevalue): 创建一个 value 的浅拷贝。

## 深拷贝

深拷贝是对对象以及对象的所有子对象进行拷贝。我们可以使用 lodash 的 `cloneDeep` 方法，但是深拷贝大数据的时候，由于内部实现是递归，所以最好换实现思路。

### `JSON.parse(JSON.stringify)` 深拷贝的局限

1. 如果被拷贝的对象中有 `function`，则拷贝之后的对象就会**丢失**这个 `function`
2. 如果被拷贝的对象中有正则表达式，则拷贝之后的正则表达式会变成 `Object`

```js
let a = {name: 'youngjuning', sayName: function() {alert(this.name)}, numberReg: /\d+/}
let b = JSON.parse(JSON.stringify(a))
// sayName丢失，numberReg变为了对象
console.dir(b) // {name: 'youngjuning', numberReg: {}}
```

## 深拷贝与浅拷贝的区别

- 深拷贝：将 B 对象拷贝到 A 对象中，包括 B 里面的子对象，
- 浅拷贝：将 B 对象拷贝到 A 对象中，但不包括 B 里面的子对象
