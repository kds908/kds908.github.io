---
title: js语法收集
tag: js
date: 2021-7-17
---
# 判断执行
```js
if(el){
 query(el)
}
el && query(el)

if(el){
 el = query(el)
}
el = el && query(el);
if(!el) return
el = query(el)

// 是否存执行方法  或者读取数据
let object = {
  name:'张三',
  hobby:{
    a:'钓鱼',
    b:'滑板'
  }
}
// 意思是如果object存在就取hobby(否则终止),如果hobby存在就取a(否则终止)。当有一个不存在的时候就取|| 后面的默认值
let a = object?.hobby?.a || '暂无数据'

// || 与 ??
// ?? 只会过 null 与 undefined
// || 当数据为 '', 0, null, undefined, false 都会走默认值
axios.get('/...').then(res => {
  if(res.code === 200) {
    this.data = res.data || {}
  }
})
axios.get('/...').then(res => {
  if(res.code === 200) {
    this.data = res.data ?? {}
  }
})

```
# 少用三目运算符
这个我个人理解是如果判断的东西非常简单，就是用三目(判断有两种情况，对应两种值、或者两个方法)
如果情况来判断比较多 就使用if（判断的东西有三种情况，对应三种）
如果更多的话就使用switch
```js
let whether = true
whether ？ this.yes() : this.no()

let index = 0
if (index < 0) {
  this.beLessThan()
} else if(index === 0){
  this.equal()
} else {
  this.moreThanThe()
}

switch (index) {
  case 0:
    // ...
    break;
  case 1:
    // ...
    break;
  case 2:
    // ...
    break;
  case 3:
    // ...
    break;
  case 4:
    // ...
    break;
  default:
    // ...
    break;
}
```

# 传参使用默认值
```js
function logicProcessingFunction( name = 'defaults' ) {
    // ...
}

```
# 使用 Object.assign 设置默认属性
```js
const menuConfig = {
  title: 'Order',
  body: 'Send'
};
function createMenu(config) {
  config = Object.assign({
    title: 'Foo',
    body: 'Bar'
  }, config);
  // config : {title: "Order", body: "Bar"}
  // ...
}
createMenu(menuConfig);
```
# 函数参数( 最好 2 个或更少 )，如果参数超过两个，建议使用 ES6 的解构语法，不用考虑参数的顺序。
```js
function logicProcessingFunction({name, age, gender, height, weight, hobby = '钓鱼'}) {
  console.log(name, age, gender, height, weight, hobby)
}
let data = {
  name: 'zhangSan', age:3 , gender: '男', height:180, weight: 80
}
logicProcessingFunction(data)
```
# 每个方法只做一件事情
在处理一个需求的时候，将需求拆分细化：
例如，第一步、第二步、第三步
或者，第一种情况、第二种情况、第三种情况
这样有利于维护,处理事件的逻辑不会混乱
```js
var getUserInfo = function(){
    ajax( 'http:// xxx.com/userInfo', function( data ){
        console.log( 'userId: ' + data.userId );
        console.log( 'userName: ' + data.userName );
        console.log( 'nickName: ' + data.nickName );
    });
};

//改成：
var getUserInfo = function(){
    ajax( 'http:// xxx.com/userInfo', function( data ){
        printDetails( data );
    });
};

var printDetails = function( data ){
    console.log( 'userId: ' + data.userId );
    console.log( 'userName: ' + data.userName );
    console.log( 'nickName: ' + data.nickName );
};
```
# 把条件分支语句提炼成函数
```js
var getPrice = function( price ){
    var date = new Date();
    if ( date.getMonth() >= 6 && date.getMonth() <= 9 ){    // 夏天
        return price * 0.8;
    }
    return price;
};

// 改成
var isSummer = function(){
    var date = new Date();
    return date.getMonth() >= 6 && date.getMonth() <= 9;
};

var getPrice = function( price ){
    if ( isSummer() ){    // 夏天
        return price * 0.8;
    }
    return price;
};
```
# 类
```js
// bad
function Person(name, age){
    this.name = name;
    this.age = age;
}
 
Person.prototype.addAge = function(){
    this.age++;
};
 
Person.prototype.setName = function(name){
    this.name = name;
};
// good
class Person{
    constructor(name, age){
        this.name = name;
        this.age = age;
    }
    addAge(){
        this.age++;
    }
    setName(name){
        this.name = name;
    }
}
```
# 尽量不要写全局方法
```js
// Bad:
Array.prototype.diff = function diff(comparisonArray) {
  const hash = new Set(comparisonArray);
  return this.filter(elem => !hash.has(elem));
};
// Good:
class SuperArray extends Array {
  diff(comparisonArray) {
    const hash = new Set(comparisonArray);
    return this.filter(elem => !hash.has(elem));        
  }
}
```
