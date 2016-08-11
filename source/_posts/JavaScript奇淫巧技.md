---
title: JavaScript奇淫巧技
date: 2016-08-10 14:14:02
tags: [JavaScript]
categories: 杂项
---

本文整理自网络，所有代码都经过V8 5.2.361.49测试，不定时更新
<!-- more -->
# 首次为变量赋值时务必使用var关键字
变量没有声明而直接赋值得话，默认会作为一个新的全局变量，要尽量避免使用全局变量。
# 使用`===` 代替` ==`
`==`和`!=`操作符会在需要的情况下自动转换数据类型。但`===`和`!==`不会，它们会同时比较值和数据类型，这也使得它们要比`==`和`!=`快。
# `undefined`、`null`、`0`、`false`、`NaN`、`空字符串`的逻辑结果均为false
# `typeof`、`instanceof`和`contructor`
```javascript
var arr = ["a", "b", "c"];
typeof arr;   // 返回 "object" 
arr instanceof Array // true
arr.constructor();  //[]
```
# 自执行匿名函数/立即调用函数表达式
函数在创建之后直接自动执行，通常称之为自执行匿名函数（Self-Invoked Anonymous Function）或立即调用函数表达式（Immediately Invoked Function Expression ）。格式如下：
```javascript
( function() {}() );
( function() {} )();
[ function() {}() ];

~ function() {}();
! function() {}();
+ function() {}();
- function() {}();

delete function() {}();
typeof function() {}();
void function() {}();
new function() {}();
new function() {};

var f = function() {}();

1, function() {}();
1 ^ function() {}();
1 > function() {}();
// ...
```
# 从数组中随机获取成员
```javascript
var items = [12, 548 , 'a' , 2 , 5478 , 'foo' , 8852, , 'Doe' , 2145 , 119];
var  randomItem = items[Math.floor(Math.random() * items.length)];
```
# 获取指定范围内的随机数
```javascript
var x = Math.floor(Math.random() * (max - min + 1)) + min;
```
# 生成从0到指定值的数字数组
```javascript
var numbersArray = [] , max = 100;
for( var i=1; numbersArray.push(i++) < max;);  // numbersArray = [1,2,3 ... 100]
```
# 生成随机的字母数字字符串
```javascript
function generateRandomAlphaNum(len) {
    var rdmString = "";
    for( ; rdmString.length < len; rdmString  += Math.random().toString(36).substr(2));
    return  rdmString.substr(0, len);
}
//generateRandomAlphaNum(50)=>"xzts8w4sf7id8yvf936m9529inj3nl1mf8bz7dbad6sp833di3"
```
# 打乱数字数组的顺序
```javascript
var numbers = [5, 458 , 120 , -215 , 228 , 400 , 122205, -85411];
numbers = numbers.sort(function(){ return Math.random() - 0.5});
/* numbers 数组将类似于 [120, 5, 228, -215, 400, 458, -85411, 122205]  */
```
# 数组之间追加
```javascript
var array1 = [12 , "foo" , {name "Joe"} , -2458];
var array2 = ["Doe" , 555 , 100];
Array.prototype.push.apply(array1, array2);
/* array1 值为  [12 , "foo" , {name "Joe"} , -2458 , "Doe" , 555 , 100] */
```
# 对象转换为数组
```javascript
var argArray = Array.prototype.slice.call(arguments);
```
# 验证是否是数组
```javascript
function isArray(obj){
    return Object.prototype.toString.call(obj) === '[object Array]' ;
}
```
但如果toString()方法被重写过得话，就行不通了。也可以使用下面的方法：
```javascript
Array.isArray(obj); // its a new Array method
```
# 获取数组中的最大值和最小值
```javascript
var  numbers = [5, 458 , 120 , -215 , 228 , 400 , 122205, -85411]; 
var maxInNumbers = Math.max.apply(Math, numbers); 
var minInNumbers = Math.min.apply(Math, numbers);
```
# 保留指定小数位数
```javascript
var num =2.443242342;
num = num.toFixed(4);  // num will be equal to 2.4432
```
注意，`toFixec()`返回的是字符串，不是数字。
# 浮点计算的问题
```javascript
0.1 + 0.2 === 0.3 // is false 
9007199254740992 + 1 // is equal to 9007199254740992
9007199254740992 + 2 // is equal to 9007199254740994
```
为什么呢？因为0.1+0.2等于0.30000000000000004。JavaScript的数字都遵循IEEE 754标准构建，在内部都是64位浮点小数表示，具体可以参见[JavaScript中的数字是如何编码的](http://www.2ality.com/2012/04/number-encoding.html).
可以通过使用`toFixed()`和`toPrecision()`来解决这个问题。
# 通过for-in循环检查对象的属性
下面这样的用法，可以防止迭代的时候进入到对象的原型属性中。
```javascript
for (var name in object) {  
    if (object.hasOwnProperty(name)) { 
        // do something with name
    }  
}
```
# 逗号操作符
```javascript
var a = 0; 
var b = ( a++, 99 ); 
console.log(a);  // a will be equal to 1 
console.log(b);  // b is equal to 99
```
# 提前检查传入isFinite()的参数
```javascript
isFinite(0/0) ; // false
isFinite("foo"); // false
isFinite("10"); // true
isFinite(10);   // true
isFinite(undefined);  // false
isFinite();   // false
isFinite(null);  // true，这点当特别注意
```
# HTML字段转换函数
```javascript
function escapeHTML(text) {  
    var replacements= {"<": "&lt;", ">": "&gt;","&": "&amp;", "\"": "&quot;"};                      
    return text.replace(/[<>&"]/g, function(character) {  
        return replacements[character];  
    }); 
}
```
# 处理WebSocket的超时
```javascript
var timerID = 0; 
function keepAlive() { 
    var timeout = 15000;  
    if (webSocket.readyState == webSocket.OPEN) {  
        webSocket.send('');  
    }  
    timerId = setTimeout(keepAlive, timeout);  
}  
function cancelKeepAlive() {  
    if (timerId) {  
        cancelTimeout(timerId);  
    }  
}
```
`keepAlive()`函数可以放在WebSocket连接的`onOpen()`方法的最后面，`cancelKeepAlive()`放在`onClose()`方法的最末尾。

---
>感谢：
>- [不可能不确定-JavaScript奇技淫巧45招](https://chensd.com/2015-01/45-useful-javascript-tips-tricks-and-best-practices.html)
>- [JavaScript 匿名函数有哪几种执行方式? - 回答作者: 长天之云](http://zhihu.com/question/20249179/answer/14487857)

