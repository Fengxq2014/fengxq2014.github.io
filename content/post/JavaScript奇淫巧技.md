+++
draft = false
date = "2016-08-10T14:14:02+08:00"
title = "JavaScript奇淫巧技"
tags = ["JavaScript"]
topics = ["杂项"]
+++

本文整理自网络，所有代码都经过V8 5.2.361.49测试，不定时更新
<!--more-->
## 首次为变量赋值时务必使用var关键字
变量没有声明而直接赋值得话，默认会作为一个新的全局变量，要尽量避免使用全局变量。
## 使用`===` 代替` ==`
`==`和`!=`操作符会在需要的情况下自动转换数据类型。但`===`和`!==`不会，它们会同时比较值和数据类型，这也使得它们要比`==`和`!=`快。
## `undefined`、`null`、`0`、`false`、`NaN`、`空字符串`的逻辑结果均为false
## `typeof`、`instanceof`和`contructor`
```javascript
var arr = ["a", "b", "c"];
typeof arr;   // 返回 "object" 
arr instanceof Array // true
arr.constructor();  //[]
```
## 自执行匿名函数/立即调用函数表达式
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
## 从数组中随机获取成员
```javascript
var items = [12, 548 , 'a' , 2 , 5478 , 'foo' , 8852, , 'Doe' , 2145 , 119];
var  randomItem = items[Math.floor(Math.random() * items.length)];
```
## 获取指定范围内的随机数
```javascript
var x = Math.floor(Math.random() * (max - min + 1)) + min;
```
## 生成从0到指定值的数字数组
```javascript
var numbersArray = [] , max = 100;
for( var i=1; numbersArray.push(i++) < max;);  // numbersArray = [1,2,3 ... 100]
```
## 生成随机的字母数字字符串
```javascript
function generateRandomAlphaNum(len) {
    var rdmString = "";
    for( ; rdmString.length < len; rdmString  += Math.random().toString(36).substr(2));
    return  rdmString.substr(0, len);
}
//generateRandomAlphaNum(50)=>"xzts8w4sf7id8yvf936m9529inj3nl1mf8bz7dbad6sp833di3"
```
## 打乱数字数组的顺序
```javascript
var numbers = [5, 458 , 120 , -215 , 228 , 400 , 122205, -85411];
numbers = numbers.sort(function(){ return Math.random() - 0.5});
/* numbers 数组将类似于 [120, 5, 228, -215, 400, 458, -85411, 122205]  */
```
## 数组之间追加
```javascript
var array1 = [12 , "foo" , {name "Joe"} , -2458];
var array2 = ["Doe" , 555 , 100];
Array.prototype.push.apply(array1, array2);
/* array1 值为  [12 , "foo" , {name "Joe"} , -2458 , "Doe" , 555 , 100] */
```
## 对象转换为数组
```javascript
var argArray = Array.prototype.slice.call(arguments);
```
## 验证是否是数组
```javascript
function isArray(obj){
    return Object.prototype.toString.call(obj) === '[object Array]' ;
}
```
但如果toString()方法被重写过得话，就行不通了。也可以使用下面的方法：
```javascript
Array.isArray(obj); // its a new Array method
```
## 获取数组中的最大值和最小值
```javascript
var  numbers = [5, 458 , 120 , -215 , 228 , 400 , 122205, -85411]; 
var maxInNumbers = Math.max.apply(Math, numbers); 
var minInNumbers = Math.min.apply(Math, numbers);
```
## 保留指定小数位数
```javascript
var num =2.443242342;
num = num.toFixed(4);  // num will be equal to 2.4432
```
注意，`toFixec()`返回的是字符串，不是数字。
## 浮点计算的问题
```javascript
0.1 + 0.2 === 0.3 // is false 
9007199254740992 + 1 // is equal to 9007199254740992
9007199254740992 + 2 // is equal to 9007199254740994
```
为什么呢？因为0.1+0.2等于0.30000000000000004。JavaScript的数字都遵循IEEE 754标准构建，在内部都是64位浮点小数表示，具体可以参见[JavaScript中的数字是如何编码的](http://www.2ality.com/2012/04/number-encoding.html).
可以通过使用`toFixed()`和`toPrecision()`来解决这个问题。
## 通过for-in循环检查对象的属性
下面这样的用法，可以防止迭代的时候进入到对象的原型属性中。
```javascript
for (var name in object) {  
    if (object.hasOwnProperty(name)) { 
        // do something with name
    }  
}
```
## 逗号操作符
```javascript
var a = 0; 
var b = ( a++, 99 ); 
console.log(a);  // a will be equal to 1 
console.log(b);  // b is equal to 99
```
## 提前检查传入isFinite()的参数
```javascript
isFinite(0/0) ; // false
isFinite("foo"); // false
isFinite("10"); // true
isFinite(10);   // true
isFinite(undefined);  // false
isFinite();   // false
isFinite(null);  // true，这点当特别注意
```
## HTML字段转换函数
```javascript
function escapeHTML(text) {  
    var replacements= {"<": "&lt;", ">": "&gt;","&": "&amp;", "\"": "&quot;"};                      
    return text.replace(/[<>&"]/g, function(character) {  
        return replacements[character];  
    }); 
}
```
## 处理WebSocket的超时
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

## 一行代码调试css层
来看看下面的这段代码，它来自于谷歌“名猿”Addy Osmani在几天前贴出的一段代码，它的作用是用来调试你的CSS层。全部代码只有三行，但是你绝对可以把它放在一行里面完成：
```javascript
[].forEach.call($$("*"),function(a){
  a.style.outline="1px solid #"+(~~(Math.random()*(1<<24))).toString(16)
})
```
在你的Chrome浏览器的控制台中输入这段代码，你会发现不同HTML层都被使用不同的颜色添加了一个高亮的边框。是不是非常酷？但是，简单来说，这段代码只是首先获取了所有的页面元素，然后使用一个不同的颜色为它们添加了一个1ps的边框。想法很简单，但是真要实现起来却不是那么容易的一件事。
### 选择页面中所有的元素
第一件事情是获取页面中所有的元素，在上面的代码中，Addy使用了一个Chrome浏览器中特有的函数$$。你可以在你的Chrome浏览器控制台中输入$$('a')，然后你就能得到一个当前页面中所有锚元素的列表。
$$函数是许多现代浏览器命令行API中的一个部分，它等价于document.querySelectorAll,你可以将一个CSS选择器作为这个函数的参数，然后你就能够获得当前页面中所有匹配这个CSS选择器的元素列表。如果你在浏览器控制台以外的地方，你可以使用document.querySelectorAll('*')来代替$$('*')。更多关于$$函数的详细内容可以查看这个[stackoverflow](http://stackoverflow.com/questions/8981211/what-is-the-source-of-the-double-dollar-sign-selector-query-function-in-chrome-f#answer-10308917)。

当然，除了使用$$函数之外，我们还有一种更简单的方法，document.all，虽然这并不是一种很规范的使用方法，但是它几乎在每一个浏览器中都能运行成功，更多请查看[Mathias Bynens](https://mathiasbynens.be/)。
### 迭代所有的元素
经过第一步，我们已经获得了页面内所有的元素，现在我们想做的事情是遍历每一个元素，然后为它们添加一个彩色边边框。但是上面的代码究竟是怎么一回事呢？
```javascript
[].forEach.call( $$('*'), function( element ) { /* 在这里修改颜色 */ });
```
首先，我们通过选择器获得的列表是一个NodeLists对象，它和javascript中的数组有点像，你可以使用方括号来获取其中的节点，你也可以检查它其中包含多少个元素，但是它并没有实现数组包含的所有方法，因此我们并不能使用$$('*').forEach()来进行迭代。在javascript中，有好几个类似于数组但是并不是数组的对象，除了前面的NodeLists，还有函数的参数集合arguments，在这里我们可以使用call或apply函数将函数的方法运用到这些对象上。例如下面的例子：
```javascript
function say(name) {
 console.log( this + ' ' + name );
}

say.call( 'hola', 'Mike' ); // 打印 'hola Mike'
```
// 你也可以将这种方法有用在arguments对象上 function example( arg1, arg2, arg3 ) { return Array.prototype.slice.call(arguments, 1); // Returns [arg2, arg3] }

在Addy的代码中，使用了[].forEach.call而不是Array.prototype.forEach.call，二者等价，但是前者可以节省几个字节。
### 为元素添加颜色
为了让元素都有一个漂亮的边框，我们在上面的代码中使用了CSS属性outline。outline属性位于CSS盒模型之外，因此它并不影响元素的属性或者元素在布局中的位置，这对于我们来说非常有用。这个属性和修改border属性非常类似，因此下面的代码应该不会很难理解：
```javascript
a.style.outline="1px solid #" + color
```
真正有趣的地方在于定义颜色部分：
```javascript
~~(Math.random()*(1<<24))).toString(16)
```
天呐，上面的代码是什么意思？在javascript中，位操作符并不是经常被使用，因此这里可能会让很多程序员感到很疑惑。

我们想达到的目的是活的一个十六进制格式的颜色例如白色对应的是FFFFFF，蓝色对应的是0000FF，或者随便一个颜色37f9ac。虽然我们人类喜欢十进制，但是我们的代码常常会需要十六进制的东西。

我们首先要学会如何使用toString函数将一个十进制的数组转换为一个十六进制整数。这个函数可以接受一个参数，如果参数缺省，默认为十进制，但是你完全可以使用别的数组：
```javascript
(30).toString(); // "30"
(30).toString(10); // "30"
(30).toString(16); // "1e" 十六进制
(30).toString(2); // "11110" 二进制
(30).toString(36); // "u" 36是允许的最大参数值
```
除此之外，你可以使用parseInt函数将十六进制数字转换为十进制。
```javascript
parseInt("30"); // "30"
parseInt("30", 10); // "30"
parseInt("1e", 16); // "30"
parseInt("11110", 2); // "30"
parseInt("u", 36); // "30"
```
因此，我们现在只需要一个位于0和ffffff之间的十六进制数，由于:
```javascript
parseInt("ffffff", 16) == 16777215
```
而这里的16777215实际上是2^24-1。

如果你对二进制数学熟悉的话，你可能会知道1<<24 == 16777216。

再进一步，你每在1后面添加一个0，你就相当于多做了一次2的乘方：
```javascript
1 // 1 == 2^0
100 // 4 == 2^2
10000 // 16 == 2^4
1000000000000000000000000 // 16777216 == 2^24
```
因此，在这里我们可以知道Math.random()*(1<<24)表示一个位于0和16777216之间的数。

但是这里并没有结束，因为Math.random返回的是一个浮点数，但是我们只想要整数部分。我们的代码中使用波浪号操作符来完成这件事。波浪操作符在javascript中被用来对一个变量进行取反。

但是我们在这里并不关心取反，我们指向获取整数部分。因此我们还可以知道两次取反可以去掉一个浮点数的小数部分，因此~~的作用相当于parseInt：
```javascript
var a = 12.34, // ~~a = 12
    b = -1231.8754, // ~~b = -1231
    c = 3213.000001 // ~~c = 3213
;

~~a == parseInt(a, 10); // true
~~b == parseInt(b, 10); // true
~~c == parseInt(c, 10); // true
```
当然，我们还有一种更加简洁的方法，使用或操作符：
```javascript
~~a == 0|a == parseInt(a, 10)
~~b == 0|b == parseInt(b, 10)
~~c == 0|c == parseInt(c, 10)
```
最终，我们获得了一个位于0和16777216之间的随机整数，也就是我们想要的随机颜色。此时我们只需要使用toString(16)将它转化为十六进制数即可。

## 变量转换
```javascript
var myVar   = "3.14159",
str     = ""+ myVar,//  to string
int     = ~~myVar,  //  to integer
float   = 1*myVar,  //  to float
bool    = !!myVar,  /*  to boolean - any string with length
and any number except 0 are true */
array   = [myVar];  //  to array
```
但是转换日期(new Date(myVar))和正则表达式(new RegExp(myVar))必须使用构造函数，创建正则表达式的时候要使用/pattern/flags这样的简化形式。　

## 取整同时转换成数值型
```javascript
//字符型变量参与运算时，JS会自动将其转换为数值型（如果无法转化，变为NaN）
'10.567890' | 0
//结果: 10
//JS里面的所有数值型都是双精度浮点数,因此，JS在进行位运算时，会首先将这些数字运算数转换为整数，然后再执行运算
//| 是二进制或， x|0 永远等于x；^为异或，同0异1，所以 x^0 还是永远等于x；至于~是按位取反，搞了两次以后值当然是一样的
'10.567890' ^ 0        
//结果: 10
- 2.23456789 | 0
//结果: -2
~~-2.23456789
//结果: -2
```

## 日期转数值
```javascript
//JS本身时间的内部表示形式就是Unix时间戳，以毫秒为单位记录着当前距离1970年1月1日0点的时间单位
var d = +new Date(); //1295698416792
```

## 类数组对象转数组
```javascript
var arr =[].slice.call(arguments)
```
下面的实例用的更绝
```javascript
function test() {
    var res = ['item1', 'item2']
    res = res.concat(Array.prototype.slice.call(arguments)) //方法1
    Array.prototype.push.apply(res, arguments)              //方法2
}
```

## 进制之间的转换
```javascript
(int).toString(16); // converts int to hex, eg. 12 => "C"
(int).toString(8);  // converts int to octal, eg. 12 => "14"
parseInt(string,16) // converts hex to int, eg. "FF" => 255
parseInt(string,8) // converts octal to int, eg. "20" => 16
```

## 将一个数组插入另一个数组指定的位置
```javascript
var a = [1,2,3,7,8,9];
var b = [4,5,6];
var insertIndex = 3;
a.splice.apply(a, Array.prototype.concat(insertIndex, 0, b));
```

## 删除数组元素
```javascript
var a = [1,2,3,4,5];
a.splice(3,1);           //a = [1,2,3,5]
```
大家也许会想为什么要用splice而不用delete,因为用delete将会在数组里留下一个空洞，而且后面的下标也并没有递减。

## 判断是否为IE
```javascript
return "ActiveXObject" in window
```
## 不用第三方变量交换两个变量的值
```javascript
a=[b, b=a][0];
//或者
a^=b, b^=a, a^=b;
```

---
>感谢：

>- [不可能不确定-JavaScript奇技淫巧45招](https://chensd.com/2015-01/45-useful-javascript-tips-tricks-and-best-practices.html)
>- [JavaScript 匿名函数有哪几种执行方式? - 回答作者: 长天之云](http://zhihu.com/question/20249179/answer/14487857)
>- [Learning much javascript from one line of code](http://arqex.com/939/learning-much-javascript-one-line-code)

