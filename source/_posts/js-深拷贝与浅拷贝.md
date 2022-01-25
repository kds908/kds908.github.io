---
title: 浅拷贝与深拷贝
date: 2021-7-17
tag: js
type: js
---

## 浅拷贝与深拷贝 (深拷贝和浅拷贝是只针对Object和Array这样的引用数据类型的)

**浅拷贝只复制指向某个对象的指针而不复制对象本身，新旧对象还是共享同一块内存。**
**但深拷贝会另外创造一个一模一样的对象，新对象跟原对象不共享内存，修改新对象不会改到原对象。**


## 赋值和浅拷贝的区别

+ 当我们把一个对象赋值给一个新的变量时，赋的其实是该对象的在栈中的地址，而不是堆中的数据。也就是两个对象指向的是同一个存储空间，无论哪个对象发生改变，其实都是改变的存储空间的内容，因此，两个对象是联动的。
+ 浅拷贝是按位拷贝对象，它会创建一个新对象，这个对象有着原始对象属性值的一份精确拷贝。如果属性是基本类型，拷贝的就是基本类型的值；如果属性是内存地址（引用类型），拷贝的就是内存地址 ，因此如果其中一个对象改变了这个地址，就会影响到另一个对象。即：默认拷贝构造函数只是对对象进行浅拷贝复制(逐个成员依次拷贝)，即只复制对象空间而不复制资源。
  

## 浅拷贝的实现方式

```js
 var obj1 = {
    'name' : 'zhangsan',
    'age' :  '18',
    'language' : [1,[2,3],[4,5]],  //Array
};
 var obj3 = shallowCopy(obj1);
 obj3.name = "lisi";
 obj3.language[1] = ["二","三"];
 function shallowCopy(src) {
    var dst = {};
    for (var prop in src) {
        if (src.hasOwnProperty(prop)) {
            dst[prop] = src[prop];
        }
    }
    return dst;
}
console.log('obj1',obj1)
console.log('obj3',obj3)
```

**.Object.assign()**
Object.assign() 方法可以把任意多个的源对象自身的可枚举属性拷贝给目标对象，然后返回目标对象。但是 Object.assign()进行的是浅拷贝，拷贝的是对象的属性的引用，而不是对象本身。

```js
var obj = { a: {a: "kobe", b: 39} };
var initalObj = Object.assign({}, obj);
initalObj.a.a = "wade";
console.log(obj.a.a); //wade
```
  *注意：当object只有一层的时候，是深拷贝*

**Array.prototype.concat()**
```js
let arr = [1, 3, {
    username: 'kobe'
    }];
let arr2=arr.concat();
arr2[2].username = 'wade';
console.log(arr);
```

## 深拷贝的实现方式

**1.JSON.parse(JSON.stringify())**
```js
let arr = [1, 3, {
  username: ' kobe'
}];
let arr4 = JSON.parse(JSON.stringify(arr));
arr4[2].username = 'duncan';
console.log(arr, arr4)
```
*原理： 用JSON.stringify将对象转成JSON字符串，再用JSON.parse()把字符串解析成对象，一去一来，新的对象产生了，而且对象会开辟新的栈，实现深拷贝。*
**这种方法虽然可以实现数组或对象深拷贝,但不能处理函数**

**2.手写递归方法**
```js
    //定义检测数据类型的功能函数
function checkedType(target) {
  return Object.prototype.toString.call(target).slice(8, -1)
}
//实现深度克隆---对象/数组
function clone(target) {
  //判断拷贝的数据类型
  //初始化变量result 成为最终克隆的数据
  let result, targetType = checkedType(target)
  if (targetType === 'object') {
    result = {}
  } else if (targetType === 'Array') {
    result = []
  } else {
    return target
  }
  //遍历目标数据
  for (let i in target) {
    //获取遍历数据结构的每一项值。
    let value = target[i]
    //判断目标结构里的每一值是否存在对象/数组
    if (checkedType(value) === 'Object' ||
      checkedType(value) === 'Array') { //对象/数组里嵌套了对象/数组
      //继续遍历获取到value值
      result[i] = clone(value)
    } else { //获取到value值是基本的数据类型或者是函数。
      result[i] = value;
    }
  }
  return result
}
```

**3.函数库lodash**
该函数库也有提供_.cloneDeep用来做 Deep Copy
```js
var _ = require('lodash');
var obj1 = {
    a: 1,
    b: { f: { g: 1 } },
    c: [1, 2, 3]
};
var obj2 = _.cloneDeep(obj1);
console.log(obj1.b.f === obj2.b.f);
// false
```

 