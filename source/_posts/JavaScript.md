---
title: JavaScript
date: 2020-03-16 16:02:10
mathjax: true
categories: DevOps
tags:
  - JavaScript
  - Web
  - JS
---


参考:

- wiki: <https://zh.wikipedia.org/zh-cn/JavaScript>
- W3school: <https://www.w3school.com.cn/js/index.asp>
- 廖雪峰：<https://www.liaoxuefeng.com/wiki/1022910821149312>

<br/>

环境：

- ELRH7x86_64












<br/>
<br/>

---

<!--more-->

<br/>
<br/>

























# 简介

Introduction


JavaScript(JS)是一种解释型的高级编程语言。JavaScript是一门基于原型、函数先行的语言，是一门多范式的语言，它支持面向对象编程，命令式编程，以及函数式编程。它提供语法来操控文本、数组、日期以及正则表达式等，不支持I/O，比如网络、存储和图形等，但这些都可以由它的宿主环境提供支持。它已经由ECMA（欧洲电脑制造商协会）通过ECMAScript实现语言的标准化。它被世界上的绝大多数网站所使用，也被世界主流浏览器（Chrome、IE、Firefox、Safari、Opera）支持。

虽然JavaScript与Java这门语言不管是在名字上，或是在语法上都有很多相似性，但这两门编程语言从设计之初就有很大的不同。为什么起名叫JavaScript？原因是当时Java语言非常红火，所以网景公司希望借Java的名气来推广，但事实上JavaScript除了语法上有点像Java，其他部分基本上没啥关系。

JavaScript是世界上最流行的脚本语言，因为你在电脑、手机、平板上浏览的所有的网页，以及无数基于HTML5的手机App，交互逻辑都是由JavaScript驱动的。随着HTML5在PC和移动端越来越流行，JavaScript变得更加重要了。并且，新兴的Node.js把JavaScript引入到了服务器端，JavaScript已经变成了全能型选手。


<br/>
<br/>



## ECMAScript


为了让JavaScript成为全球标准，几个公司联合ECMA（European Computer Manufacturers Association）组织定制了JavaScript语言的标准，被称为ECMAScript标准。

所以简单说来就是，ECMAScript是一种语言标准，而JavaScript是网景公司对ECMAScript标准的一种实现。 JavaScript的标准是ECMAScript 。ECMAScript第一版标准发布于1997年。

那为什么不直接把JavaScript定为标准呢？因为JavaScript是网景的注册商标。












<br/>
<br/>

---

<br/>
<br/>


















# 快速入门


JavaScript代码可以直接嵌在网页的任何地方，不过通常我们都把JS代码放到`<head>`中。由`<script>...</script>`包含的代码就是JS代码，它将直接被浏览器执行。

```html
<html>
<head>
  <script>
    alert("hello, world");
  </script>
</head>
</html>
```

第二种方法是把JavaScript放到单独的`.js`文件，然后在HTML中通过`<script src="..."></script>`来引入。

```html
<html>
<head>
  <script src="/static/js/hello.js"></script>
</head>
</html>
```

将JS代码放入单独的文件中更有利于维护代码，并且多个页面可以复用。

在同一个页面中引入多个JS文件（或编写多个JS代码），浏览器将按照顺序依次执行。

有时会看到`<script>`有一个`type`属性。但其实这是没有必要的，以你为默认的`type`就是`javascript`，所以不必显式指定。

```html
<script type="text/javascript">
...
</script>
```

<br>

`console.log()`代替`alert()`的好处是可以避免弹出烦人的对话框。

```js
// 在chrome中console中查看
var x = 100;
console.log(x)
```



<br/>
<br/>



## 如何运行JS


要让浏览器运行JavaScript，必须先有一个HTML页面，在HTML页面中引入JavaScript。然后，然浏览器加载该HTML页面，就可以执行JavaScript代码。




<br/>
<br/>



## 基本语法


每个语句以分号`;`结束，语句块使用花括号`{}`。

```js
// 不建议一行写多个语句
var x = 1;
var y = 2;

/* 缩进不是JS语法要求所必须，但有助于我们理解代码层次。
   缩进通常是4个空格 */
if (x > y) {
    x = y;
}
```




<br/>
<br/>



## 数据类型


- 数字(Number)
- 字符串(String)
- 布尔(Bool)
- 空(Null)
- 未定义(Undefined)
- Symbol: 独一无二的值
- 数组(Array)
- 对象(Object)
- 函数(Function)

<br>

JS不区分整数和浮点数，统一用Number表示。
`NaN`这个特殊的数字与所有其它值都不相等，包括它自己。

字符串以单引号或双引号括起来的任意文本。可通过转义字符(\)进行转义。

布尔值只有`true`和`false`两种值。

`null`表示一个空值，如Python的`None`。`undefined`表示未定义。大多数情况下，我们都应该使用`null`，`undefined`仅仅在判断函数参数是否传递的情况下有用。
`''`表示长度为0的字符串。

数组使用`[]`表示，元素间用逗号`,`分隔。类似于Python的List，包括所索引、切片等操作。

对象是一组由键值组成的无序集合。类似于Python的Dictionary。

```js
var person = {
    name: 'A',
    age: 20,
    tags: ['js', 'html', 'css'],
    hasCar: true,
    zipCode: null
}
```


<br/>
<br/>


### Map和Set

JavaScript的默认对象表示方式`{}`可以视为其他语言中的Map或Dictionary的数据结构，即一组键值对。但是JavaScript的对象有个小问题，就是键必须是字符串。但实际上Number或者其他数据类型作为键也是非常合理的。

为了解决这个问题，最新的ES6规范引入了新的数据类型`Map`。

```js
// Map
var m = new Map([['A': 90], ['b': 80]]);
m.get('A');
m.set('A', 99);
m.has('B');
m.delete('B');
```

<br>

Set和Map类似，也是一组Key的集合，但不存储Value。没有重复的键。

```js
// Set
var s1 = new Set();
var s2 = new Set([1, 2, 3, 1, 4, 'A'])
s2.add(4)
s2.delete('A')
```


<br/>
<br/>


### 动态类型

JavaScript拥有动态类型，这意味着相同的变量可哦你工作不同的类型。

```js
var x;  // x为unfefined
var x = 5;  // x为数字
var x = "John";  // x为字符串
```


<br/>
<br/>


### 运算符

- `&&`
- `||`
- `!`
- `>`, `<`
- `>=`, `<=`
- `==`: 会自动转换数据类型再比较
- `===`: 不会自动转换数据类型，如果数据类型不一致，返回false；如果一致，再比较

由于JS这个设计缺陷，不要使用`==`，始终坚持使用`===`。




<br/>
<br/>
<br/>




## 变量


变量在JavaScript中就是用一个变量名表示，变量名是大小写英文、数字、`$`和`_`的组合，且不能用数字开头。变量名也不能是JavaScript的关键字。变量名也可以用中文。

```js
// 注意，只能用var申明一次
var a = 123;
a = 'ABC';
/* 变量本身类型不固定的语言称为动态语言
   与之相反的是静态语言，在定义变量时必须指定变量类型，如果类型不匹配，则会报错
   动态语言更灵活 */
```



可使用关键字`new`来声明变量类型:

```js
var name = new String;
var x = new Number;
var y = new Boolean;
var z = new Array;
var o = new Object;
```


<br/>
<br/>


### 严格模式


JavaScript在设计之初，为了便于学习，并不强制要求使用`var`声明变量。这个设计错误带来了严重的后果：如果一个变量没有通过`var`申明就被使用，那么该变量就自动被申明为全局变量。

```js
// i现在是全局变量
i = 10;
```

在同一个页面的不同的JavaScript文件中，如果都不用`var`申明，恰好都使用了变量i，将造成变量i互相影响，产生难以调试的错误结果。
使用`var`申明的变量则不是全局变量，它的范围被限制在该变量被申明的函数体内，同名变量在不同的函数体内互不冲突。

为了修补JavaScript这一严重设计缺陷，ECMA在后续规范中推出了严格(strict)模式，在strict模式下运行的JavaScript代码，强制通过`var`申明变量，未使用`var`申明变量就使用的，将导致运行错误。
不用`var`申明的变量会被视为全局变量，为了避免这一缺陷，所有的JavaScript代码都应该使用strict模式。我们在后面编写的JavaScript代码将全部采用strict模式。

启用strict模式的方法是在JavaScript代码的第一行写上：`'use strict';`




<br/>
<br/>
<br/>




## 条件判断


```js
if () {
    xxx;
} else {
    xxxx;
}


if (condition) {
    xx;
} else if (conditon) {
    xxx;
} else {
    xxxx;
}
```

<br>

栗子：

```js
'use strict';

var age = 20;

if (age>=18) {
    console.log('adult');
} else if (age < 6) {
    console.log('kid');
} else {
    console.log('teenager');
}
```

<br>

JavaScript把`null`, `undefined`, `0`, `NaN`, `''`视为`fasle`，其它一概视为`true`。

<br>

栗子：

```js
'use strict';

// 类似于alert的弹窗输入
var height = parseFloat(prompt('请输入身高(m):'));
var weight = parseFloat(prompt('请输入体重(kg):'));

var bmi = weight / height*height;
console.log(bmi);

if (bmi < 18.5) {
    console.log("过轻");
} else if (25 > bmi >= 18.5) {
    console.log("正常");
} else if (32 > bmi >= 25) {
    console.log("过重");
} else {
    console.log("肥胖");
}
```




<br/>
<br/>




## 循环


- `for`
- `while`
- `do...while`

<br>

```js
// for
var x = 0;
var i;

for (i=1; i<=100; i++) {
    x = x + i;
}

console.log(x)


// for 索引
var arr = ['0', '1', '2'];
var i, x;

for (i=0; i<arr.length; i++) {
    x = arr[i];
    console.log(x);
}


// >for in
for (var i in arr) {
    console.log(i);
    console.log(a[i]);
}
```

<br>

```js
// while
var x = 0;
var n = 99;

while (n > 0) {
    x = x + n;
    n = n -2;
}

console.log(x)
```

<br>

```js
// do...while
var n = 0;

do {
    n = n + 1;
} while (n < 100);  // >哈哈

console.log(n);
```




<br/>
<br/>
<br/>




## iterable

遍历`Array`可以采用下标循环，遍历`Map`和`Set`就无法使用下标。为了统一集合类型，ES6标志引入了新的`iterable`类型，Array, Map, Set都属于`iterable`类型。

具有`iterable`类型的集合可以通过新的`for...of`循环来遍历。

```js
var a = ['A', 'B', 'C'];
var s = new Set(['A', 'B', 'C']);
var m = new Map([[1, 'x'], [2, 'y'], [3, 'z']]);
for (var x of a) { // 遍历Array
    console.log(x);
}
for (var x of s) { // 遍历Set
    console.log(x);
}
for (var x of m) { // 遍历Map
    console.log(x[0] + '=' + x[1]);
}
```













<br/>
<br/>

---

<br/>
<br/>













# 函数

Function


借助抽象，我们才能不关心底层的具体计算过程，而直接在更高的层次上思考问题。写计算机程序也是一样，函数就是最基本的一种代码抽象的方式。


<br/>
<br/>


## 函数定义


JavaScript中，定义函数的方式如下：

```js
function abs(x) {
    if (x >= 0) {
        return x;
    } else {
        return -x;
    }
}


// 匿名函数
// 注意赋值语句结束需要;
var abs = function (x) {
    if (x >= 0) {
        return x;
    } else {
        return -x;
    }
};


// 调用函数
abs(10);


/* 由于JS允许传入任意个参数而不影响调用，
   因此传入的参数比定义的参数多也没有问题。
   虽然函数内部并不需要这些参数。 */
abs(10, 'haha', null);
```

<br>

JS还有一个免费赠送的关键字`arguments`，它只在函数内部起作用，并且永远指向当前函数的调用者传入的所有参数。
实际上`arguments`最常用于判断传入参数的个数。

```js
function abs() {
    if (arguments.length === 0) {
        return 0;
    }
    var x = arguments[0];
    return x >= 0 ? x : -x :
}
```

<br>

ES6标准引入了`rest`参数。

```js
function foo(a, b, ...rest) {
    console.log(a);
    console.log(b);
    console.log(rest);
}

foo(1, 2, 3, 4);
// 1
// 2
// Array [3, 4]
foo(1);
// 1
// undefined
// Array []
```




<br/>
<br/>
<br/>




## 变量作用域


在JavaScript中，用`var`申明的变量实际上是有作用域的。
如果一个变量在函数体内部申明，则该变量的作用域为整个函数体，在函数体外不可引用该变量。


<br/>
<br/>


### 变量提升


虽然JavaScript的函数有一个**变量提升**的特点，它会先扫描整个函数体的语句，把所有申明的变量提升到函数顶部。但我们在函数内部定义变量时，请在函数内部首先申明所有变量。


<br/>
<br/>


### 全局作用域


不在任何函数内定义的变量就具有全局作用域。实际上，JavaScript默认有一个全局对象`window`，全局作用域实际上被绑定到`window`的一个属性。

```js
var course = 'JavaScript';
alert(course);
alert(window.course);
```

这说明JavaScript实际上只有一个全局作用域。任何变量（函数也视为变量），如果没有在当前函数作用域中找到，就会继续往上查找，最后如果在全局作用域中也没有找到，则报`ReferenceError`错误。


<br/>
<br/>


### 命名空间


全局变量会绑定到`window`上，不同的JavaScript文件如果使用了相同的全局变量，或者定义了相同名字的顶层函数，都会造成命名冲突，并且很难被发现。

减少冲突的一个方法是把自己的所有变量和函数全部绑定到一个全局变量中。例如：

```js
// 唯一的全局变量MYAPP
var MYAPP = {};

MYAPP.name = 'myapp';
MYAPP.version = 1.0;

MYAPP.foo = function () {
    return 'foo';
};
```

把自己的代码全部放入唯一的命名空间`MYAPP`中，会大大减少全局变量冲突的可能。
许多著名的JS库都是这么做的：jQuery, YUI等等。


<br/>
<br/>


### 局部作用域


由于JavaScript的变量作用域实际上是函数内部，我们在`for`循环等语句块中是无法定义具有局部作用域的变量的。

为了解决块级作用域，ES6引入了新的关键字`let`，用`let`替代`var`可以申明一个块级作用域的变量。


<br/>
<br/>


### 常量


由于`var`和`let`申明的是变量，如果要申明一个常量，在ES6之前是不行的，我们通常用全部大写的变量来表示这是一个常量，不要修改它的值。

```js
var PI = 3.14;
```

ES6标准引入了新的关键字`const`来定义常量，`const`和`let`都具有块级作用域。

```js
const PI = 3.14;
PI = 3; // TypeError: Assignment to constant variable.
```




<br/>
<br/>
<br/>




## 解构赋值

从ES6开始，JavaScript引入了解构赋值，可以同时对一组变量进行赋值。

使用解构赋值可以减少代码量，但是，需要在支持ES6解构赋值特性的现代浏览器中才能正常运行。




<br/>
<br/>
<br/>




## 方法


在一个对象中绑定函数，称为这个对象的方法。

```js
var xiaoming = {
    name: 'Ming',
    birth: 1990,
    age: function () {
        var y = new Date().getFullYear();
        return y - this.birth;
    }
};
```

注意，有一个`this`关键字。在一个方法内部，`this`是一个特殊变量，它始终指向当前对象，也就是`xiaoming`这个变量。

<br>

我们可以控制`this`的指向。要确定函数的`this`指向哪个对象，可以用函数本身的`apply`方法，它接收两个参数，第一个参数就是需要绑定的`this`变量，第二个参数是`Array`，表示函数本身的参数。

另一个与`apply()`类似的方法是`call()`，唯一区别是：

- `apply()`把参数打包成`Array`再传入；
- `call()`把参数按顺序传入。

栗子：

```js
// Math.max(3, 5, 4)
Math.max.apply(null, [3, 5, 4]);
Math.max.call(null, 3, 5, 4);
```

对普通函数调用，我们通常把`this`绑定为`null`。


<br/>
<br/>


### 装饰器

利用`apply()`，我们还可以动态改变函数的行为。

JavaScript的所有对象都是动态的，即使内置的函数，我们也可以重新指向新的函数。




<br/>
<br/>
<br/>




## 高阶函数


一个函数就可以接收另一个函数作为参数，这种函数就称之为高阶函数(Higher order function)。

```js
function add(x, y, f) {
    return (f(x) + f(y));
}


add(-5, 6, Math.abs)
```


<br/>
<br/>


### map/reduce


`map()`方法定义在JS的`Array`中。

```js
function pow(x) {
    return (x * x);
}

var arr = [1, 2, 3];
var results = arr.map(pow); // [1, 4, 9]
```

<br>

`reduce()`把结果继续和序列的下一个元素做累积计算。

```js
var arr = [1, 4, 5, 7];
arr.reduce(function (x, y) {
    return (x + y);
}); // 17
```


<br/>
<br/>


### filter

`filter()`它用于把`Array`的某些元素过滤掉，然后返回剩下的元素。

```js
// 保留奇数
var arr = [1, 2, 3, 4, 5];
var r = arr.filter(function (x){
    return (x % 2 !== 0);
});
```


<br/>
<br/>


### sort

JavaScript的`Array`的`sort()`方法就是用于排序的，但它默认把所有元素先转换为String再排序，如果不知道这个，那么用它直接对数字排序会栽进坑里。


<br/>
<br/>


### 其它高阶函数

`Array`对象还提供了许多非常实用的高阶函数：

- `every()`: 判断数组的所有元素是否满足测试条件
- `find()`: 查找符合条件的第一个元素，如果找到了，返回这个元素；否则，返回`undefined`
- `findIndex()`: 它返回查找元素的索引
- `forEach`: 把每个元素依次作用于传入的函数，但不会返回新的数组。常用于遍历数组



<br/>
<br/>
<br/>



## 闭包

闭包是一种保护私有变量的机制，在函数执行时形成私有的作用域，保护里面的私有变量不受外界干扰。直观的说就是形成一个不销毁的栈环境。



<br/>
<br/>
<br/>



## 箭头函数

ES6新增了一种新的函数：**箭头函数(Arrow Function)**。
感觉有点类似于Python的lambda。

```js
// arrow function
x => x * x

// 相当于
function (x) {
    return x * x;
}
```

<br>

其它用法:

```js
// 两个参数
(x, y) => (x * x) + (y * y)

// 无参数
() => 3.14

// 可变参数
(x, y, ...rest) => {
    xxx;
}


// 返回对象
x => ({foo: x})
```



<br/>
<br/>
<br/>



## 生成器

**生成器(generator)**是ES6标准引入的新的数据类型。一个生成器看上去像一个函数，但可以返回多次。
同样类似于Python的生成器，还记得`next`和`yield`吗？哈哈。

```js
// generator 斐波那契数列
function* fib(max) {
    var
         t,
         a = 0,
         b = 1,
         n = 0;
    while (n < max) { //>haha
        yield a;
        [a, b] = [b, a+b];
        n++;
    }
    return;
}
```

直接调用生成器和调用函数不一样，仅仅是创建了一个生成器对象，还没有去执行它。

```
// 第一种方法：不断调用生成器对象的next()方法，需要判断是否done
var f = fib(5);
f.next();
f.next();
...
f.next()


// 第二种方法：for ... of循环迭代生成器对象
for (var x of fib(10)) {
    console.log(x);
}
```












<br/>
<br/>

---

<br/>
<br/>




















# 标准对象

在JavaScript的世界里，一切都是对象。但某些对象还是和其它对象不一样。

```js
// typeof 获取对象类型
typeof 123; // 'number'
typeof null; // 'object'
typeof []; // 'object'
```

JS还提供了包装对象，熟悉Java的小伙伴肯定很清楚`int`和`Integer`这种暧昧关系。
虽然包装对象看上去和原来的值一摸一样，但是它们的类型已经变为`object`了。所以，闲的蛋疼也不要使用包装对象。

```js
// 包装对象用 new 创建
var n = new Number(123);
var b = new Boolean(true);

typeof new Number(123); // 'object'
new Number(123) === 123; // false
```



<br/>
<br/>



## Date

在JavaScript中，`Date`对象用来表示日期和时间。
注意，当前时间是浏览器从本机操作系统获取的时间，所以不一定准确，因为用户可以把当前时间设定为任何值。

> JavaScript的Date对象月份值从0开始，牢记0=1月，1=2月，2=3月，……，11=12月。

```js
var now = new Date();
now; // Wed Jun 24 2015 19:49:22 GMT+0800 (CST)
now.getFullYear(); // 2015, 年份
now.getMonth(); // 5, 月份，注意月份范围是0~11，5表示六月
now.getDate(); // 24, 表示24号
now.getDay(); // 3, 表示星期三
now.getHours(); // 19, 24小时制
now.getMinutes(); // 49, 分钟
now.getSeconds(); // 22, 秒
now.getMilliseconds(); // 875, 毫秒数
now.getTime(); // 1435146562875, 以number形式表示的时间戳


// 创建一个日期对象
var d = new Date(2015, 5, 19, 20, 15, 30, 123);
d; // Fri Jun 19 2015 20:15:30 GMT+0800 (CST)

// 或ISO 8601格式
var d = Date.parse('2015-06-24T19:49:22.875+08:00');
d; // 1435146562875

// 或时间戳
var d = new Date(1435146562875);
d; // Wed Jun 24 2015 19:49:22 GMT+0800 (CST)
d.getMonth(); // 5
```

<br>

**时区**

`Date`对象表示的时间总是按浏览器所在的时区显示，不过我们既可以显示本地时间，也可以显示调整后的UTC时间。

```js
var d = new Date(1435146562875);
d.toLocaleString(); // '2015/6/24 下午7:49:22'，本地时间（北京时区+8:00），显示的字符串与操作系统设定的格式有关
d.toUTCString(); // 'Wed, 24 Jun 2015 11:49:22 GMT'，UTC时间，与本地时间相差8小时
```



<br/>
<br/>



## RegExp

强大的正则表达式！

了解了基本的RE知识，我们就可以在JavaScript中使用正则表达式了。JS有两种方式创建一个正则表达式：

- `/正则表达式/`
- `new RegExp('正则表达式')`，创建一个RegExp对象

```js
var re1 = /ABC\-001/;
var re2 = new RegExp('ABC\\-001'); // 使用了转义字符


// 测试正则
var re = /^$d{3}\-\d{3, 8}$/;
re.test('010-12345'); // true
```

<br>

**切分字符串**

用正则表达式切分字符串比用固定的字符更灵活。

```js
'a b   c'.split(' '); // ['a', 'b', '', '', 'c']

'a b   c'.split(/\s+/); // ['a', 'b', 'c']
```

<br>

**分组**

除了简单地判断是否匹配之外，正则表达式还有提取子串的强大功能。用`()`表示的就是要提取的分组（Group）。

如果正则表达式中定义了组，就可以在RegExp对象上用`exec()`方法提取出子串来。

```js
var re = /^(\d{3})-(\d{3,8})$/;
re.exec('010-12345'); // ['010-12345', '010', '12345']
re.exec('010 12345'); // null
```

<br>

**贪婪匹配**

需要特别指出的是，正则匹配默认是贪婪匹配，也就是匹配尽可能多的字符。

```js
// 贪婪匹配
var re = /^(\d+)(0*)$/;
re.exec('102300'); // ['102300', '102300', '']

// 非贪婪匹配
var re = /^(\d+?)(0*)$/;
re.exec('102300'); // ['102300', '1023', '00']
```

<br>

**全局搜索**

JavaScript的正则表达式还有几个特殊的标志，最常用的是`g`，表示全局匹配。

```js
var r1 = /test/g;
// 等价于:
var r2 = new RegExp('test', 'g');
```



<br/>
<br/>



## JSON

JSON是JavaScript Object Notation的缩写，它是一种轻量级的数据交换格式。

在JSON出现之前，大家一直用XML来传递数据。因为XML是一种纯文本格式，所以它适合在网络上交换数据。XML本身不算复杂，但是，加上DTD、XSD、XPath、XSLT等一大堆复杂的规范以后，任何正常的软件开发人员碰到XML都会感觉头大了，最后大家发现，即使你努力钻研几个月，也未必搞得清楚XML的规范。

JSON实际上是JavaScript的一个子集。在JSON中，一共就这么几种数据类型：

- number
- boolean
- string
- null
- array
- object

JSON还定死了字符集必须是UTF-8，表示多语言就没有问题了。为了统一解析，JSON的字符串规定必须用双引号`""`，Object的键也必须用双引号`""`。

由于JSON非常简单，很快就风靡Web世界，并且成为ECMA标准。几乎所有编程语言都有解析JSON的库，而在JavaScript中，我们可以直接使用JSON，因为JavaScript内置了JSON的解析。

把任何JavaScript对象变成JSON，就是把这个对象序列化成一个JSON格式的字符串，这样才能够通过网络传递给其他计算机。
如果我们收到一个JSON格式的字符串，只需要把它反序列化成一个JavaScript对象，就可以在JavaScript中直接使用这个对象了。

<br>

**序列化**

```js
// JavaScript object to JSON
var xiaoming = {
    name: 'xiaoming',
    age: 14,
    gender: true,
    height: 1.65,
    grade: null,
    skills: ['JS', 'Java', 'Python']
};

var s = JSON.stringift(xiaoming);
console.log(s);
```

<br>

**反序列化**

```js
// JSON to JavaScript object
JSON.parse('{"name":"小明","age":14}'); // Object {name: '小明', age: 14}
```














<br/>
<br/>

---

<br/>
<br/>





















# 面向对象编程

JavaScript的面向对象编程和大多数其他语言如Java、C#的面向对象编程都不太一样。面向对象的两个基本概念：

- 类：类是对象的模板
- 实例：根据类创建的对象

所以，类和实例是大多数面向对象编程语言的基本概念。

不过，在JavaScript中，这个概念需要改一改。JavaScript不区分类和实例的概念，而是通过原型（prototype）来实现面向对象编程。
JavaScript没有类的概念，所有对象都是实例。所谓继承关系不过是把一个对象的原型指向另一个对象而已。



<br/>
<br/>


## 创建对象

JavaScript对每个创建的对象都会设置一个原型，指向它的原型对象。

当我们用`obj.xxx`访问一个对象的属性时，JavaScript引擎先在当前对象上查找该属性，如果没有找到，就到其原型对象上找，如果还没有找到，就一直上溯到`Object.prototype`对象，最后，如果还没有找到，就只能返回`undefined`。

```js
// 创建Array对象
var arr = [1, 2, 3];

// 原型链
arr ----> Array.prototype ----> Object.prototype ----> null
```

<br>

**构造函数**

JavaScript还可以用一种构造函数的方法来创建对象。

```js
// 定义一个构造函数
function Student(name) {
    this.name = name;
    this.hello = function () {
        alert('Hello, ' + this.name + '!');
    }
}

// 这确实是一个普通函数，但在JavaScript中，可以用关键字new来调用这个函数，并返回一个对象
// 写了new， 它就变成了一个构造函数
var xiaoming = new Student('小明');
xiaoming.name;
xiaoming.hello();


// xiaoming的原型链
xiaoming ----> Student.prototype ----> Object.prototype ----> null
```

要让创建的对象共享一个函数，根据对象的属性查找规则，我们只需要把此函数移动到这些对象的原型上就可以了，也就是`xxx.prototype`:

```js
function Student(name) {
    this.name = name;
}

Student.prototype.hello = function() {
    alert('Hello, ' + this.name + '!');
}
```

为了区分普通函数和构造函数，按照约定，构造函数首字母应当大写，而普通函数首字母应当小写。



<br/>
<br/>



## 原型继承

在传统的基于类的语言中，继承的本质是扩展一个已有的类(class)，并生成新的子类(subclass)。

但是，由于JavaScript采用原型继承，我们无法直接扩展一个类，因为根本不存在类。

JavaScript的原型继承方式是：

- 定义新的构造函数，并在内部用`call()`调用希望继承的构造函数，并绑定`this`
- 借助中间函数`F`实现原型继承，最好通过封装的`inherits`函数完成
- 继续在新的构造函数的原型上定义新方法


```js
function Student(props) {
    this.name = props.name || 'Unnamed';
}

Student.prototype.hello = function () {
    alert('Hello, ' + this.name + '!');
}


// PrimaryStudent构造函数:
function PrimaryStudent(props) {
    Student.call(this, props);
    this.grade = props.grade || 1;
}

// 空函数F:
function F() {
}

// 把F的原型指向Student.prototype:
F.prototype = Student.prototype;

// 把PrimaryStudent的原型指向一个新的F对象，F对象的原型正好指向Student.prototype:
PrimaryStudent.prototype = new F();

// 把PrimaryStudent原型的构造函数修复为PrimaryStudent:
PrimaryStudent.prototype.constructor = PrimaryStudent;

// 继续在PrimaryStudent原型（就是new F()对象）上定义方法：
PrimaryStudent.prototype.getGrade = function () {
    return this.grade;
};


// 创建xiaoming:
var xiaoming = new PrimaryStudent({
    name: '小明',
    grade: 2
});
xiaoming.name; // '小明'
xiaoming.grade; // 2

// 验证原型:
xiaoming.__proto__ === PrimaryStudent.prototype; // true
xiaoming.__proto__.__proto__ === Student.prototype; // true

// 验证继承关系:
xiaoming instanceof PrimaryStudent; // true
xiaoming instanceof Student; // true
```



<br>
<br>



## class继承

JavaScript的对象模型是基于原型实现的，特点是简单，缺点是理解起来比传统的类－实例模型要困难，最大的缺点是继承的实现需要编写大量代码，并且需要正确实现原型链。

但有更简单的写法。在ES6标准中，新的关键字`class`正式被引入到JavaScript中。它的目的就是让定义类更简单。

```js
// class的定义包含了构造函数constructor
class Student {
    constructor(name) {
        this.name = name;
    }
    hello() {
        alert('Hello, ' + this.name + '!');
    }
}


// 创建对象
var xiaoming = new Student('小明');
xiaoming.hello();
```

用`class`定义对象的另一个巨大好处是继承更方便了。不需要写大量的中间对象，直接通过`extends`来实现：

```js
class PrimaryStudent extends Student {
    constructor(name, grade) {
        super(name); // 记得用super调用父类的构造方法!
        this.grade = grade;
    }
    myGrade() {
        alert('I am at grade ' + this.grade);
    }
}
```

要注意浏览器是不是支持ES6的class。


























<br/>
<br/>

---

<br/>
<br/>
















# 浏览器

由于JavaScript的出现就是为了能在浏览器中运行，所以，浏览器自然是JavaScript开发者必须要关注的。

目前，主流浏览器分这几种：

- IE 6-11：从IE 10开始支持ES6；
- Chrome：Google出品的基于Webkit内核，内置非常强悍的JS引擎——V8。支持ES6；
- Safari：Mac自带的基于Webkit内核，支持ES6；
- Firefox：Mozilla研制的Gecko内核和JS引擎OdinMonkey。支持ES6；
- 移动设备(IOS/Android)：支持ES6

<br>

不同的浏览器对JavaScript支持的差异主要是，有些API的接口不一样，比如AJAX，File接口。对于ES6标准，不同的浏览器对各个特性支持也不一样。

在编写JavaScript的时候，就要充分考虑到浏览器的差异，尽量让同一份JavaScript代码能运行在不同的浏览器中。



<br/>
<br/>



## 浏览器对象

JavaScript可以获取浏览器提供的很多对象，并进行操作。


<br/>


### window

`window`对象不但充当全局作用域，而且表示浏览器窗口。属性：

- `innerWidth`：浏览器窗口的内部宽度
- `innerHeight`：内部高度
- `outerWidth`: 浏览器窗口整个宽度
- `outerHeight`: 整个高度

内部宽高是指除去菜单栏、工具栏、边框等占位元素后，用于显示网页的净宽高。

```js
// 可以调整浏览器窗口大小，进行测试
cosole.log('window inner size: ' + window.innerWidth + 'x' + window.innerHeight);
```


<br/>
<br/>


### navigator

`navigator`表示浏览器的信息，最常用的属性包括：

- `appName`：浏览器名称；
- `appVersion`：浏览器版本；
- `language`：浏览器设置的语言；
- `platform`：操作系统类型；
- `userAgent`：浏览器设置的User-Agent字符；

请注意，navigator的信息可以轻易的被用户修改。

```js
console.log('appName = ' + navigator.appName);
console.log('appVersion = ' + navigator.appVersion);
console.log('language = ' + navigator.language);
console.log('platform = ' + navigator.platform);
console.log('userAgent = ' + navigator.userAgent);
```


<br/>
<br/>


### screen

`screen`对象表示屏幕的信息，常用的属性有：

- `width`：屏幕宽度(px)；
- `height`：屏幕高度(px)；
- `colorDepth`：颜色位数；

```js
console.log('Screen size = ' + screen.width + ' x ' + screen.height);
```


<br/>
<br/>


### location

`location`对象表示当前页面的URL信息。

- `href`：完整的URL
- `protocol`：协议
- `host`: 主机名
- `port`：端口
- `pathname`： 路径
- `search`： 参数
- `hash`：
- `assign()`：加载一个新页面
- `reload()`：重载当前页面

```js
if (confirm('重新加载当前页' + location.href + '?')) {
    location.reload();
} else {
    location.assign('/'); // 设置一个新的URL地址
}
```


<br/>
<br/>


### document

`document`对象表示当前页面。由于HTML在浏览器中以DOM形式表示为树形结构，`document`对象就是整个DOM树的根节点。

- `title`
- `getElementById()`：按id获得一个DOM节点；
- `getElementsByTagName()`：按tag获得一个DOM节点；
- `getElementsByClassName()`
- `cookie`：为了确保安全，服务器端在设置Cookie时，应该始终坚持使用httpOnly；

```js
// document.title属性从HTML文档中<title>xxx</title>读取，但可以动态改变
document.title = 'DOM title';
```


<br/>
<br/>


### history

`history`保存了浏览器的历史记录，JavaScript可以调用`history()`对象的`back()`或`forward()`。
这个对象属于历史遗留问题，任何情况，你都不应该使用`history`这个对象。



<br/>
<br/>
<br/>



## DOM


**DOM(文档对象模型, document object model)**，是W3C(万维网联盟)的标准。它定义了访问HTML和XML文档的标准。

由于HTML文档被浏览器解析后就是一棵DOM树，要改变HTML的结构，就需要通过JavaScript来操作DOM。

<br>

DOM节点有几个操作：

- 更新
- 遍历
- 添加
- 删除

在操作一个DOM节点前，我们需要通过各种方式先拿到这个DOM节点。常用的方法：

- `document.getElementById()`：由于ID在HTML文档中是唯一的，所以可以直接定位唯一的一个DOM节点。
- `document.getElementsByTagName()`：返回一组DOM节点
- `document.getElementsByClassName()`：要精确地选择DOM，可以先定位父节点，再从父节点开始选择，以缩小范围
- `document.querySelector()`
- `document.queryAelectorAll()`


<br/>
<br/>


### 更新DOM

拿到DOM节点后，我们可以对它进行更新。可以直接修改节点文本，方法有两种：

一种是修改`innerHTML`属性，这个方式非常强大，不但可以修改一个DOM节点的文本内容，还可以直接通过HTML片段修改DOM节点内部的子树。
用innerHTML时要注意，是否需要写入HTML。如果写入的字符串是通过网络拿到了，要注意对字符编码来避免XSS攻击。

```js
// 获取<p id="p-id">...</p>
var p = document.getElementById('p-id');
// 设置文本为abc:
p.innerHTML = 'ABC'; // <p id="p-id">ABC</p>
// 设置HTML:
p.innerHTML = 'ABC <span style="color:red">RED</span> XYZ';
// <p>...</p>的内部结构已修改
```

第二种是修改`innerText`或`textContent`属性，这样可以自动对字符串进行HTML编码，保证无法设置任何HTML标签。

```js
// 获取<p id="p-id">...</p>
var p = document.getElementById('p-id');
// 设置文本:
p.innerText = '<script>alert("Hi")</script>';
// HTML被自动编码，无法设置一个<script>节点:
// <p id="p-id">&lt;script&gt;alert("Hi")&lt;/script&gt;</p>
```

两者的区别在于读取属性时，innerText不返回隐藏元素的文本，而textContent返回所有文本。因为CSS允许`font-size`这样的名称，但它并非JavaScript有效的属性名，所以需要在JavaScript中改写为驼峰式命名`fontSize`。

<br>

修改CSS也是经常需要的操作。DOM节点的`style`属性对应所有的CSS，可以直接获取或设置。

```js
// 获取<p id="p-id">...</p>
var p = document.getElementById('p-id');
// 设置CSS:
p.style.color = '#ff0000';
p.style.fontSize = '20px';
p.style.paddingTop = '2em';
```


<br/>
<br/>


### 插入DOM

如果DOM节点是空的，那么，直接使用`innerHTML = <x>aaa</x>`就可以修改DOM节点的内容，相当于插入了新的DOM节点。

如果DOM节点不是空，则不能这样做。有两个新办法。

一个是使用`appendChild`：把一个子节点添加到父节点的最后一个子节点。

```js
var
    js = document.getElementById('js'),
    list = document.getElementById('list);
list.append(js);
```

```js
// 更多的时候，我们会从零创建一个新的节点，然后插入到指定位置
var
    list = document.getElementById('list'),
    haskell = document.createElement('p');
haskell.id = 'haskell';
haskell.innerText = 'Haskell';
list.appendChild(haskell);
```

动态创建一个节点，然后田间道DOM树中，可以实现很多功能。

```js
var d = document.createElement('style');
d.setAttribute('type', 'text/css');
d.innerHTML = 'p {color: red}';
document.getElementsByTagName('head')[0].appendChild(d);
```

这个栗子更改了颜色。可在浏览器的console上执行来看效果。

<br>

**insertBefore**

使用`parentElement.insertBefore(newElement, referenceElement);`，子节点会插入到`referenceElement`之前。

```js
var
    list = document.getElementById('list'),
    ref = document.getElementById('python'),
    haskell = document.createElement('p');
haskell.id = 'haskell';
haskell.innerText = 'Haskell';
list.insertBefore(haskell, ref);
```

可见，使用`insertBefore`重要的是拿到一个参考子节点的引用。很多时候，需要循环一个父节点的所有字节嗲，可以通过`children`属性实现：

```js
var
    i, c,
    list = document.getElementById('list');
for (i = 0; i < list.children.length; i++) { //>haha
    c = list.children[i];
}
```


<br/>
<br/>


### 删除DOM

要删除一个节点，首先要获得该节点本身以及它的父节点。然后，调用父节点的`removeChild`把自己删掉。

```js
// 拿到待删除节点:
var self = document.getElementById('to-be-removed');
// 拿到父节点:
var parent = self.parentElement;
// 删除:
var removed = parent.removeChild(self);
removed === self; // true
```

注意，删除后的节点虽不在文档树中，但其实它还在内存中，可以随时在此被添加到别的位置。

当你遍历一个父节点的子节点并进行删除操作时，要注意，`children`属性是一个只读属性，并且它在子节点变化时会实时更新。因此，删除多个节点时，要注意`children`属性时刻都在变化。



<br/>
<br/>
<br/>



## 操作表单

用JavaScript操作表单和操作DOM是类似的，因为表单本身也是DOM树。

不过，表单的输入框、下拉框等可以接收用户输入，所以用JavaScript来操作表单，可以获得用户输入的内容，或者对一个输入框设置新的内容。

HTML表单的输入控件主要有以下几种：

- 文本框：`<input type="text">`;
- 口令框：`<input type="password">`;
- 单选框：`<input type="radio">`;
- 复选框：`<input type="checkbox">`;
- 下拉框：`<select>`;
- 隐藏文本，用户不可见，但表单提交时会把隐藏文本发送到服务器: `input type="hidden">`。


<br/>
<br/>


### 获取值

如果我们获取了一个`<input>`节点的引用，就可以调用`value`获得对应的用户输入值。

```js
// <input type="text" id="email">
var input = document.getElementById('email');
input.value; // '用户输入的值'
```

这种方式可以用于text, password, hidden, select。但是，对于单选框和复选框，`value`属性返回的永远是HTML预设的值，而我们需要获得的实际是用户是否勾上了选项，所以应该用`checked`判断。

```js
// <label><input type="radio" name="weekday" id="monday" value="1"> Monday</label>
// <label><input type="radio" name="weekday" id="tuesday" value="2"> Tuesday</label>
var mon = document.getElementById('monday');
var tue = document.getElementById('tuesday');
mon.value; // '1'
tue.value; // '2'
mon.checked; // true或者false
tue.checked; // true或者false
```


<br/>
<br/>


### 设置值

对于text, password, hidden, select，直接设置`value`就可以。对于单/复选框，设置`checked`为`true`或`false`即可。

```js
// <input type="text" id="email">
var input = document.getElementById('email');
input.value = 'test@example.com'; // 文本框的内容已更新
```


<br/>
<br/>


### HTML5控件

HTML5新增了大量标准空间，常用的包括date, datetime, datetime-local, color...，它们都使用`<input>`标签。

不支持HTML5的浏览器无法识别新的控件，会把它们当做`type="text"`来显示。支持HTML5的浏览器将获得格式化的字符串。


<br/>
<br/>


### 提交表单

最后，JavaScript可以以两种方式来处理表单的提交（AJAX方式在后面介绍）。

一是通过`<form>`元素的`submit()`方法提交一个表单。这种方式的缺点是扰乱了浏览器对form的正常提交。

```html
<!-- html -->
<form id="test-form">
    <input type="text" name="test">
    <button type="button" onclick="doSubmitForm()">Submit</button>
</form>

<script>
function doSubmitForm() {
    var form = document.getElementById('test-form');
    // 可在此修改form的input
    // 提交form
    form.submit();
}
</script>
```

第二种方式是响应`<form>`本身的`onsubmit`事件，在提交form时作修改：

```html
<!-- html -->
<form id="test-form" onsubmit="return checkForm()">
    <input type="text" name="test">
    <button type="submit">Submit</button>
</form>

<script>
function checkForm() {
    var form = document.getElementById('test-id');
    return true;
}
</script>
```

注意要`return true`来告诉浏览器继续提交，如果`return false`，浏览器将不会继续提交form，这种情况通常对应用户输入有误，提示用户错误信息后终止提交form。

在检查和修改`<input>`时，要充分利用`<input type="hidden">`来传递数据。例如，很多登录表单希望用户输入的口令（出于安全考虑）在提交表单时不传输明文口令，而是口令的MD5。

```html
<!-- html -->
<form id="login-form" method="post" onsubmit="return checkForm()">
    <input type="text" id="username" name="username">
    <input type="password" id="password" name="password">
    <button type="submit">Submit</button>
</form>

<script>
function checkForm() {
    var pwd = document.getElementById('password');
    // to MD5
    pwd.value = toMD5(pwd.value);
    return true;
}
</script>
```

这样做看上去没问题，但用户输入了口令提交时，口令框突然会从几个`*`变为32个`**`（MD5有32个字符）。若不想改变用户的输入，可利用`<input type="hidden">`实现：

```html
<!-- HTML -->
<form id="login-form" method="post" onsubmit="return checkForm()">
    <input type="text" id="username" name="username">
    <input type="password" id="input-password">
    <input type="hidden" id="md5-password" name="password">
    <button type="submit">Submit</button>
</form>

<script>
function checkForm() {
    var input_pwd = document.getElementById('input-password');
    var md5_pwd = document.getElementById('md5-password');
    // 把用户输入的明文变为MD5:
    md5_pwd.value = toMD5(input_pwd.value);
    // 继续下一步:
    return true;
}
</script>
```

没有`name`属性的`<input>`的数据不会被提交。



<br/>
<br/>
<br/>



## 操作文件

在HTML表单中，可以上传文件的唯一控件就是`<input type="file">`。

> 注意：当一个表单包含`<input type="file">`时，表单的`enctype`必须指定为`multipart/form-data`，`method`必须指定为`post`，浏览器才能正确编码并以`multipart/form-data`格式发送表单的数据。

处于安全考虑，浏览器只允许用户点击`<input type="file">`来选择本地文件，用JavaScript对`<input type="file">`的`value`赋值是没有任何效果的。当用户选择了上传某个文件后，JavaScript也无法获得该文件的真实路径。

通常，上传的文件都由后台服务器处理，JavaScript可以在提交表单时对文件的扩展名做检查，以便防止用户上传无效格式的文件。

```js
var f = document.getElementById('file-upload');
var filename = f.value;

if(!filename || !(filename.endsWith('.jpg')) || filename.endsWith('.png') || filename.endsWith('.gif')) {
    alert("Can only upload image file.");
    return false;
}
```

<br>

**File API**

由于JavaScript对用户上传的文件操作非常有限，尤其是无法读取文件内容，使得很多需要操作文件的网页不得不用Flash这样的第三方插件来实现。

随着HTML5的普及，新增的File API允许JavaScript读取文件内容，获得更多的文件信息。HTML5的File API提供了`File`和`FileReader`两个主要对象，可以获得文件信息并读取文件。

<br>

**回调**

在JavaScript中，浏览器的JavaScript执行引擎在执行JavaScript代码时，总是以单线程模式执行，也就是说，任何时候，JavaScript代码都不可能同时有多于1个线程在执行。

你可能会问，单线程模式执行的JavaScript，如何处理多任务？在JavaScript中，执行多任务实际上都是异步调用。因为是异步操作，所以我们在JavaScript代码中就不知道什么时候操作结束，因此需要先设置一个回调函数：

```js
reader.onload = function(e) {
    // 操作完成后，自动调用此函数
};
```



<br/>
<br/>
<br/>



## AJAX

AJAX(Asynchronous JavaScript and XML)不是JavaScript的规范，意思是用JavaScript执行异步网络请求。

如果仔细观察一个Form的提交，你就会发现，一旦用户点击Submit按钮，表单开始提交，浏览器就会刷新页面，然后在新页面里告诉你操作是成功了还是失败了。如果不幸由于网络太慢或者其他原因，就会得到一个404页面。

这就是Web的运作原理：一次HTTP请求对应一个页面。

如果要让用户留在当前页面中，同时发出新的HTTP请求，就必须用JavaScript发送这个新请求，接收到数据后，再用JavaScript更新页面，这样一来，用户就感觉自己仍然停留在当前页面，但是数据却可以不断地更新。

最早大规模使用AJAX的就是Gmail，Gmail的页面在首次加载后，剩下的所有数据都依赖于AJAX来更新。

用JavaScript写一个完整的AJAX代码并不复杂，但是需要注意：AJAX请求是异步执行的，也就是说，要通过回调函数获得响应。

```js
// 在现代浏览器上写AJAX主要依靠XMLHttpRequest对象
function success(text) {
    var textarea = document.getElementById('response-text');
    textarea.value = text;
}

function fail(code) {
    var textarea = document.getElementById('response-text');
    textarea.value = 'Error code: ' + code;
}

var request = new XMLHttpRequest(); // 新建XMLHttpRequest对象

request.onreadystatechange = function () {
    // 状态发生变化时，函数被回调
    if (rquest.readyState === 4) {
        // 判断相应结果
        if (request.status === 200) {
            return success(request.responseText);
        } else {
            return fail(request.status);
        }
    } else {
        // HTTP请求还在继续
    }
}


// 发送请求
request.open('GET', '/api/categories');
request.send();

alert('请求已发送，请等待响应...');
```

<br>

**安全限制**

上面代码的URL使用的是相对路径。如果你把它改为`http://xxx.com/`，再运行，肯定会报错。在console里，还可以看到错误信息。

这是因为浏览器的同源策略导致的。默认情况下，JavaScript在发送AJAX请求时，URL的域名必须和当前页面完全一致。

那是不是JavaScript无法请求外域（其它网站）的URL了呢？方法还是有的：

- 一是通过Flash插件发送HTTP请求，这种方式可以绕过浏览器的安全限制，但必须安装Flash，并且跟Flash交互。不过Flash用起来麻烦，而且现在用得也越来越少了。
- 二是通过在同源域名下架设一个代理服务器来转发，JavaScript负责把请求发送到代理服务器。代理服务器再把结果返回，这样就遵守了浏览器的同源策略。这种方式麻烦之处在于需要服务器端额外做开发。
- 三是JSONP。它有个限制，只能用GET请求，并且要求返回JavaScript。这种方式跨域实际上是利用了浏览器允许跨域引用JavaScript资源。

<br>

**CORS**

如果浏览器支持HTML5，那么就可以一劳永逸地使用新的跨域策略：CORS(Cross-Origin Resource Sharing)，它是HTML5规范定义的如何跨域访问资源。

了解CORS前，我们先搞明白概念。Origin表示本域，也就是浏览器当前页面的域。当JavaScript向外域发起请求后，浏览器收到响应后，首先检查`Access-Control-Allow-Origin`是否包含本域，如果是，则此次跨域请求成功，如果不是，则请求失败，JavaScript将无法获取到响应的任何数据。

可见，跨域能否成功，取决于对方服务器是否愿意给你设置一个正确的`Access-Control-Allow-Origin`，决定权始终在对方手中。



<br/>
<br/>
<br/>



## Promise

在JavaScript的世界中，所有代码都是单线程执行的。由于这个缺陷，导致JavaScript的所有网络操作，浏览器事件，都必须是异步执行。

异步执行可以用回调函数实现：

```js
function callback() {
    console.log('Done');
}
console.log("before setTimeout()");
setTimeout(callback, 1000); // 1s后调用callback
console.log("after setTimeout()");
```

可见，异步操作会在将来某个时间点触发一个函数调用。

Promise有各种开源实现，在ES6中被统一规范，由浏览器直接支持。

Promise最大的好处是在异步执行的流程中，把执行代码和处理结果的代码清洗地分离。

Promise还可以做更多的事情，比如，有若干个异步任务，需要先做任务1，如果成功后再做任务2，任何任务失败则不再继续并执行错误处理函数。
要串行执行这样的异步任务，不用Promise需要些一层一层的嵌套代码。

```js
// job 1, 2, 3都是Promise对象
job1.then(job2).then(job3).catch(handleError);
```

<br>

除了串行执行若干异步任务外，Promise还可以并行执行异步任务。

如果我们组合使用Promise，就可以把很多异步任务以并行和串行的方式组合起来执行。



<br/>
<br/>
<br/>



## Canvas

Canvas是HTML5新增的组件，它就像一块幕布，可以用JavaScript在上面绘制各种图标、动画等。没有Canvas的年代，绘图只能借助Flash插件实现，页面不得不用JavaScript和Flash进行交互。有了Canvas，我们就再也不需要Flash了，直接使用JavaScript完成绘制。

一个Canvas定义了一个指定尺寸的矩形框，在这个范围内我们可以随意绘制：

```js
<canvas id="test-canvas" width="300" height="200"></canvas>
```

<br>

**绘制形状**

我们可以在Canvas上绘制各种形状。在绘制前，我们需要了解以下Canvas的坐标系统。Canvas的坐标以左上角为原点，水平向右为X轴，垂直向下为Y轴，以像素为单位，所以每个点都是非负整数。

<br>

**绘制文本**

绘制文本就是在指定的位置输出文本，可以设置文本的字体、样式、阴影等，与CSS完全一致。

<br>

Canvas除了能绘制基本的形状和文本，还可以实现动画、缩放、各种滤镜和像素转换等高级操作。如果要实现非常复杂的操作，考虑以下优化方案：

- 通过创建一个不可见的Canvas来绘图，然后将最终绘制结果复制到页面的可见Canvas中；
- 尽量使用整数坐标而不是浮点数；
- 可以创建多个重叠的Canvas绘制不同的层，而不是在一个Canvas中绘制非常复杂的图；
- 背景图片如果不变可以直接用`<img>`标签并放到最底层。
















<br/>
<br/>

---

<br/>
<br/>
















# JQuery

你可能听说过jQuery，它名字起得很土，但却是JavaScript世界中使用最广泛的一个库。
江湖传言，全世界大约有80~90%的网站直接或间接地使用了jQuery。鉴于它如此流行，又如此好用，所以每一个入门JavaScript的前端工程师都应该了解和学习它。

JQuery的理念是**Write Less, Do More**，让你写更少的代码，完成更多的工作。

JQuery能帮助解决一些很重要的问题：

- 消除浏览器差异
- 简洁的操作DOM的方法：写`$('#test')`肯定比`document.getElementById('test')`来的简洁
- 轻松实现动画、修改CSS等各种操作。

<br>

**JQuery版本**

JQuery有1.x和2.x两个主要版本，区别在于2.x移除了对古老的IE6、7、8的支持，因此2.x的代码更精简。

JQuery只是一个`jquery-xxx.js`文件，但你会看到有compressed（已压缩）和uncompressed（未压缩）两种版本，使用时完全一样，但如果你想深入研究jQuery源码，那就用uncompressed版本。

<br>

**使用JQuery**

使用JQuery只需要在页面的`<head>`引入JQuery文件即可：

```html
<html>
<head>
    <script src="code.jquery.com/jquery-2.1.4.min.js"></script>
</head>

<body>
...
</body>
</html>
```

<br>

**$符号**

`$`符号是著名的JQuery符号。实际上，Jquery把所有功能全部封装在一个全局变量`JQuery`中，而`$`也是一个合法的变量名，它是`JQuery`的别名。

```js
window.jQuery; // jQuery(selector, context)
window.$; // jQuery(selector, context)
$ === jQuery; // true
typeof($); // 'function'
```

`$`本质上是一个函数，但函数也是对象。于是`$`除了可以直接调用外，也可以有很多其它属性。

> 注意：你看到的`$`函数名可能不是`JQuery(selector, context)`，因为很多JavaScript压缩工具可以对函数名和参数改名，所以压缩过的JQuery源码`$`函数可能变成`a(b, c)`。

绝大多数时候，我们都直接用`$`。但是，如果`$`这个变量不幸地被占用了，而且还不能改，那我们只能让`JQuery`把`$`变量交出来，然后就只能使用`JQuery`这个变量。

```js
$; // jQuery(selector, context)
jQuery.noConflict();
$; // undefined
jQuery; // jQuery(selector, context)
```

这种黑魔法的原理是JQuery在占用`$`之前，现在内部保存了原来的`$`，调用`JQuery.noConflict()`时会把原来保存的变量还原。


<br/>
<br/>


## 选择器

选择器是JQuery的核心，一个选择器写出来大概是这样：`$('#dom-id')`。

为什么JQuery要发明选择器？来回顾一下DOM操作中经常使用的代码：

```js
// 按ID查找
var a = document.getElementById('dom-id');

// 按tag查找
var divs = document.getElementsByTagName('div');
```

这些代码实在太过繁琐。并且在层级关系中，很多时候需要递归查找所有子节点。
JQuery的选择器就是帮助我们快速定位到一个或多个DOM节点。

<br>

**按id查找**

```js
var div = $('#abc');

// 如果不存在，则返回 []
```

它返回JQuery对象。JQuery对象类似数组，它的每个元素都是一个引用了DOM节点的对象。

JQuery的选择器不会返回`undefined`或`null`，这样的好处是不必在下一行判断`if (div === undefined)`。

```js
// JQuery对象和DOM对象直接可以互相转化
var div = $('#abc'); //JQuery对象
var divDom = div.get(0); // 假设存在div，获取第1个DOM元素
var another = $(divDom); // 重新把DOM包装为JQuery对象
```

通常情况下你不需要获取DOM对象，直接使用jQuery对象更加方便。如果你拿到了一个DOM对象，那可以简单地调用$(aDomObject)把它变成jQuery对象，这样就可以方便地使用jQuery的API了。

<id>

**按tag查找**

```js
var ps = $('p'); // 返回所有<p>节点
ps.length; // 数一数页面有多少个<p>节点
```

<id>

**按class查找**

```js
var a = $('.red'); // 所有节点包含`class="red"`都将返回

// 多个class
var a = $('.red.green'); // 注意没有空格！
```

<br>

**按属性查找**

```js
var email= $('[name=email]'); // 找出所有name属性为email的DOM

// 还可使用前缀/后缀查找
var icons = $('[name^=icon]'); // 找出所有name属性以icon开头的DOM
var names = $('[name$=with]'); // 找出所有name属性以with结尾的DOM
```

<br>

**组合查找**

组合查找就是把上述简单选择器组合起来使用。

```js
var emailInput = $('input[name=email]');

// tag and class
var tr = $('tr.red'); // 找出<tr class='red ...'>...</tr>
```

<br>

**多项选择器**

多项选择器就是把多个选择器用逗号`,`组合起来：

```js
$('p,div'); // 把<p>和<div>都选出来
$('p.red, p.green');
```


<br/>
<br/>


### 层级选择器

除了基本的选择器外，jQuery的层级选择器更加灵活，也更强大。因为DOM的结构就是层级结构，所以我们经常要根据层级关系进行选择。

<br>

**层级选择器(Descendant Selector)**

如果两个DOM元素具有层级关系，就可以用`$('ancestor descendant')`来选择，层级之间用空格隔开。

```js
$('ul.lang li.lang-javascript');

$('div.testing li.lang-python');
```

这种层级选择器相比单个的选择器好处在于，它缩小了选择范围，因为首先要定位父节点，才能选择相应的子节点，这样避免了页面其他不相关的元素。

<br>

**子选择器(Child Selector)**

子选择器`$('parent>child')`是限定了父子关系的层级选择器。

```js
$('ul.lang>li.lang-javascript');
```

<br>

**过滤器(Filter)**

过滤器一般不单独使用，它通常附加在选择器上，帮助我们更精确地定位元素。

```js
$('ul.lang li');

$('ul.lang li:first-child');
$('ul.lang li:nth-child(2)');
```

<br>

**表单相关**

针对表单元素，jQuery还有一组特殊的选择器:

- `:input`，可选择input, textarea, select, button
- `:file`，可选择`<input type="file">`，和`input[type=file]`一样
- `:checkbox`
- `:radio`
- `:focus`
- `:checked`
- `:enabled`
- `:disabled`


<br/>
<br/>


### 查找和过滤

通常情况下选择器可以直接定位到我们想要的元素，但是，当我们拿到一个jQuery对象后，还可以以这个对象为基准，进行查找和过滤。

```js
// find()查找
var ul = $('ul.lang'); // 获得<ul>
var dy = ul.find('.dy'); // 获得JavaScript, Python, Scheme
var swf = ul.find('#swift'); // 获得Swift
var hsk = ul.find('[name=haskell]'); // 获得Haskell


// 要从当前节点开始向上查找，使用parent()方法
var swf = $('#swift'); // 获得Swift
var parent = swf.parent(); // 获得Swift的上层节点<ul>
var a = swf.parent('.red'); // 获得Swift的上层节点<ul>，同时传入过滤条件。如果ul不符合条件，返回空jQuery对象


// 对于位于同一层级的节点，可以通过next()和prev()
var swift = $('#swift');

swift.next(); // Scheme
swift.next('[name=haskell]'); // 空的jQuery对象，因为Swift的下一个元素Scheme不符合条件[name=haskell]
swift.prev(); // Python
swift.prev('.dy'); // Python，因为Python同时符合过滤器条件.dy
```

```js
// filter()方法可以过滤不符合选择器条件的节点
var langs = $('ul.lang li'); // 拿到JavaScript, Python, Swift, Scheme和Haskell
var a = langs.filter('.dy'); // 拿到JavaScript, Python, Scheme
```



<br/>
<br/>
<br/>



## 操作DOM

jQuery的选择器很强大，用起来又简单又灵活，但是搞了这么久，我拿到了jQuery对象，到底要干什么？

当然是操作对应的DOM节点啦！

回顾一下修改DOM的CSS、文本、设置HTML有多么麻烦，而且有的浏览器只有innerHTML，有的浏览器支持innerText，有了jQuery对象，不需要考虑浏览器差异了，全部统一操作！

<br>

**修改Text和HTML**

jQuery对象的`text()`和`html()`方法分别获取节点的文本和原始HTML文本。

```js
// 获取
$('#ul li[name=book]').text();
$('#ul li[name=book]').html();

// 修改
var j1 = $('#test-ul li.js');
var j2 = $('#test-ul li[name=book]');

j1.html('<span style="color: red">JavaScript</span>');
j2.text('JavaScript & ECMAScript');
```

<br>

**修改CSS**

```js
// css('name', 'value')
var div = $('#test-div');
div.css('color'); // '#000033', 获取CSS属性
div.css('color', '#336699'); // 设置CSS属性
div.css('color', ''); // 清除CSS属性
```

<br>

**显示和隐藏DOM**

考虑到显示和隐藏DOM元素使用非常普遍，jQuery直接提供`show()`和`hide()`方法。

> 注意，隐藏DOM节点并未改变DOM树的结构，它只影响DOM节点的显示。这和删除DOM节点是不同的。

```js
var a = $('a[target=_blank]');
a.hide(); // 隐藏
a.show(); // 显示
```

<br>

**获取DOM信息**

利用jQuery对象的若干方法，我们直接可以获取DOM的许多信息，而无需针对不同浏览器编写特定代码。

- `width()`
- `height()`
- `attr()`：获取或修改属性
- `removeAttr()`
- `prop()`
- ...

```js
$(window).width();
$(window).height();

var div = $('#test-div');
div.width(400);
div.height('200px');

div.attr('name');
div.attr('name', 'Hello');
div.removeAttr('name');
```

<br>

**操作表单**

对于表单元素，jQuery对象统一提供`val()`方法获取和设置对应的`value`属性。一个`val()`就统一了各种输入框的取值和赋值的问题。

```js
var
    input = $('#input'),
    select = $('#select'),
    textarea = $('#textarea');
input.val();
input.val('xxx@example.com');
```


<br/>
<br/>


### 修改DOM结构

有了jQuery，我们就专注于操作jQuery对象本身，底层的DOM操作由jQuery完成就可以了，这样一来，修改DOM也大大简化了。

<br>

**添加DOM**

除了`html()`这种暴力方法外，还可以用`append()`方法。`append()`把DOM添加到最后，`prepend()`则把DOM添加到最前。

```js
var ul = $('#test-div>ul');
ul.append('<li><span>xxx</span></li>');
```

除了接受字符串，`append()`还可以传入原始的DOM对象、jQuery对象和函数对象。

```js
// 创建DOM对象:
var ps = document.createElement('li');
ps.innerHTML = '<span>Pascal</span>';
// 添加DOM对象:
ul.append(ps);

// 添加jQuery对象:
ul.append($('#scheme'));

// 添加函数对象:
ul.append(function (index, html) {
    return '<li><span>Language - ' + index + '</span></li>';
});
```

同级节点可以用`after()`或者`before()`方法。

<br>

**删除节点**

要删除DOM节点，拿到jQuery对象后直接调用`remove()`方法就可以了。如果jQuery对象包含若干DOM节点，实际上可以一次删除多个DOM节点。



<br/>
<br/>
<br/>



## 事件

因为JavaScript在浏览器中以单线程模式运行，页面加载后，一旦页面上所有的JavaScript代码被执行完后，就只能依赖触发事件来执行JavaScript代码。

浏览器在接收到用户的鼠标或键盘输入后，会自动在对应的DOM节点上触发相应的事件。如果该节点已经绑定了对应的JavaScript处理函数，该函数就会自动调用。由于不同的浏览器绑定事件的代码都不太一样，所以用jQuery来写代码，就屏蔽了不同浏览器的差异，我们总是编写相同的代码。

```js
// 栗子：点击超链接弹出提示框，用jQuery绑定一个click事件
var a = $('#test-link');
a.on('click', function () {
    alert('Hello!';)
});

// on方法用来绑定一个事件，需要传入事件名称和对应的处理函数

// 一种更简化的写法
a.click(function () {
    alert('Hello!');
});
```

<br>

jQuery能够绑定的事件主要有：

- 鼠标事件
  - click：鼠标单击时触发；
  - dblclick：鼠标双击时触发；
  - mouseenter：鼠标进入时触发；
  - mouseleave：鼠标移出时触发；
  - mousemove：鼠标在DOM内部移动时触发；
  - hover：鼠标进入和退出时触发两个函数（相当于mouseenter+mouseleave）。
- 键盘事件：仅作用在当前焦点的DOM上
  - keydown：键盘按下时触发；
  - keyup：键盘松开时触发；
  - keypress：按一次键后触发。
- 其它事件
  - focus：当DOM获得焦点时触发；
  - blur：当DOM失去焦点时触发；
  - change：当input, select, textarea的内容改变时触发；
  - submit：当form提交时触发；
  - ready：当页面被载入并且DOM树完成初始化后触发。

<br>

**取消绑定**

一个已被绑定的事件可以解除绑定，通过`off('click', function)`实现。

```js
function hello() {
    alert('hello!');
}

a.click(hello); // 绑定事件

// 10秒钟后解除绑定:
setTimeout(function () {
    a.off('click', hello);
}, 10000);
```

需要特别注意，以下这种写法无效：

```js
/ 绑定事件:
a.click(function () {
    alert('hello!');
});
// 解除绑定:
a.off('click', function () {
    alert('hello!');
});
```

这是因为两个匿名函数虽然长得一模一样，但是它们是两个不同的函数对象，`off('click', function () {...})`无法移除已绑定的第一个匿名函数。

为了实现移除效果，可以使用`off('click')`一次性移除已绑定的`click`事件的所有处理函数。
同理，无参数调用`off()`一次性移除已绑定的所有类型的事件处理函数。

<br>

**事件触发条件**

一个需要注意的问题是，事件的触发总是由用户操作引发的。

<br>

**浏览器安全限制**

在浏览器中，有些JavaScript代码只有在用户触发下才能执行。

```js
// 如window.open()函数

// 无法弹出新窗口，将被浏览器屏蔽:
$(function () {
    window.open('/');
});
```



<br/>
<br/>
<br/>



## 动画

用JavaScript实现动画，原理非常简单：我们只需要以固定的时间间隔（例如，0.1秒），每次把DOM元素的CSS样式修改一点（例如，高宽各增加10%），看起来就像动画了。

但是要用JavaScript手动实现动画效果，需要编写非常复杂的代码。如果想要把动画效果用函数封装起来便于复用，那考虑的事情就更多了。

使用jQuery实现动画，代码就非常简单了。

<br>

**jQuery内置的几种动画样式**

- `show()`：显示DOM元素，从左上角展开；
- `hiden()`：隐藏DOM元素，从左上角收缩；
- `sideUp()`：在垂直方向展开；
- `sideDown()`：在垂直反向收缩；
- `fadeIn()`：动画效果淡入；
- `fadeOut()`：动画效果淡出。

```js
var div = $('#test-show-hide');
div.hide(3000); // 在3秒钟内逐渐消失

var div = $('#test-show-hide');
div.show('slow'); // 在0.6秒钟内逐渐显示

var div = $('#test-slide');
div.slideUp(3000); // 在3秒钟内逐渐向上消失

var div = $('#test-fade');
div.fadeOut('slow'); // 在0.6秒内淡出
```

<br>

**自定义动画**

使用`animate()`可实现任意动画效果，需要传入的参数就是DOM元素最终的CSS状态和时间，jQuery在时间段内不断调整CSS直到达到设定的值。

```js
var div = $('#test-animate');
div.animate({
    opacity: 0.25,
    width: '256px',
    height: '256px'
}, 3000, function () {
    console.log('动画已结束');
    // 恢复至初始状态:
    $(this).css('opacity', '1.0').css('width', '128px').css('height', '128px');
});
```

<br>

**串行动画**

jQuery的动画效果还可以串行执行，通过`delay()`方法还可以实现暂停，这样我们可以实现更复杂的动画效果。

```js
var div = $('#test-animates');
// 动画效果：slideDown - 暂停 - 放大 - 暂停 - 缩小
div.slideDown(2000)
   .delay(1000)
   .animate({
       width: '256px',
       height: '256px'
   }, 2000)
   .delay(1000)
   .animate({
       width: '128px',
       height: '128px'
   }, 2000);
}
```



<br/>
<br/>
<br/>



## AJAX

jQuery在全局对象`jQuery`(也就是`$`)绑定了`ajax()`函数，可以处理AJAX请求。`ajax(url, settings)`常用的选项如下：

- async：是否异步执行AJAX请求，默认true，千万不要指定为false；
- method：缺省为GET；
- content type：发送POST请求的格式，默认为`application/x-www-form-urlencoded; charset=UTF-8`，也可指定为`text/plain`, `application/json`；
- data：发送的数据，可以是字符串、数组、对象；
- headers：发送的额外HTTP头，必须是一个对象；
- dataType：接收的数据格式，可指定为html, xml, json, text等。

<br>

**get**

对常用的AJAX操作，jQuery提供了一些辅助方法。由于GET请求最常见，所以jQuery提供了`get()`方法。

```js
var jqxhr = $.get('/path/to/resource', {
    name: 'xxx',
    check: 1
});

// 实际URL：/path/to/resource?name=Bob%20Lee&check=1
```

<br>

**post**

与get类似，但传入的第二个参数默认被序列化为`application/x-www-form-urlencoded`。

```js
var jqxhr = $.post('/path/to/resource', {
    name: 'Bob Lee',
    check: 1
});

// 实际构造数据：name=Bob%20Lee&check=1
```

<br>

**getJSON**

由于json越来越普遍，所以jQuery也提供了`getJSON()`方法来快速通过GET获取一个json对象。

```js
var jqxhr = $.getJSON('/path/to/resource', {
    name: 'Bob Lee',
    check: 1
}).done(function (data) {
    // data已经被解析为JSON对象了
});
```

<br>

**安全限制**

jQuery的AJAX完全封装的是JavaScript的AJAX操作，所以它的安全限制和前面讲的用JavaScript写AJAX完全一样。



<br/>
<br/>
<br/>



## 扩展

当我们使用jQuery对象的方法时，由于jQuery对象可以操作一组DOM，而且支持链式操作，所以用起来非常方便。但是jQuery内置的方法永远不可能满足所有的需求。

我们可以扩展jQuery来实现自定义方法。我们可以扩展jQuery来实现自定义方法。


<br>


### 编写jQuery插件

给jQuery对象绑定一个新方法是通过扩展`$.fn`对象实现的。

```js
$.fn.highlight1 = function () {
    // this已绑定为当前jQuery对象，所以函数内部代码可以正常调用所有jQuery对象的方法
    this.css('backgroundColor', '#fffceb').css('color', '#d85030');
    return this; // 因为jQuery对象支持链式操作，我们自己写的扩展方法也要能继续链式下去
}


// 可以给方法加个参数，让用户自己把参数用对象传进去
$.fn.highlight2 = function (options) {
    // 要考虑到各种情况:
    // options为undefined
    // options只有部分key
    var bgcolor = options && options.backgroundColor || '#fffceb';
    var color = options && options.color || '#d85030';
    this.css('backgroundColor', bgcolor).css('color', color);
    return this;
}
```

<br>

另一种方法是使用jQuery提供的辅助方法`$.extend(target, obj1, obj2, ...)`，它把多个对象的属性合并到第一个target对象中，遇到同名属性，总是使用靠后的对象的值，也就是越往后优先级越高。

紧接着用户对`highlight2()`提出了意见：每次调用都需要传入自定义的设置，能不能让我自己设定一个缺省值，以后的调用统一使用无参数的`highlight2()`？也就是说，我们设定的默认值应该能允许用户修改。那默认值放哪比较合适？放全局变量肯定不合适，最佳地点是`$.fn.highlight2`这个函数对象本身。

```js
$.fn.highlight = function (options) {
    // 合并默认值和用户设定值:
    var opts = $.extend({}, $.fn.highlight.defaults, options);
    this.css('backgroundColor', opts.backgroundColor).css('color', opts.color);
    return this;
}

// 设定默认值:
$.fn.highlight.defaults = {
    color: '#d85030',
    backgroundColor: '#fff8de'
}
```

<br>

最终，我们得出编写一个jQuery插件的原则：

- 给`$.fn`绑定函数，实现插件的代码逻辑；
- 插件函数最后要`return this;`以支持链式调用；
- 插件函数要有默认值，绑定在`$.fn.<pluginName>.defaults`上；
- 用户在调用时可传入设定值以便覆盖默认值。


<br/>
<br/>


### 针对特定元素的扩展

我们还知道jQuery对象的有些方法只能作用在特定的DOM元素上，比如`submit()`方法只能针对`form`。如果我们编写的扩展只能针对某些类型的DOM元素，应该怎么写？

还记得jQuery的选择器支持`filter()`方法来过滤吗？我们可以借助这个方法来实现针对特定元素的扩展。


















<br/>
<br/>

---

<br/>
<br/>




















# 错误处理

错误分两种：

- 一种是程序逻辑写的不对，导致代码执行异常；
- 一种是执行过程中，程序可能遇到无法预测的异常情况而报错。

错误处理是程序设计时必须要考虑的问题。

```js
// 类似于Python的try
try {
    ...
} catch (e) {
    ...
} finally {
    ...
}
```

<br>

**错误类型**

JavaScript有一个标准的`Error`对象表示错误，还有从`Error`派生的`TypeError`, `FeferenceError`等错误对象。

```js
try {
    ...
} catch (e) {
    if (e instanceof TypeError) {
        alert('Type error!');
    } else if (e instanceof Error) {
        alert(e.message);
    } else {
        alert('Error: ' + e);
    }
}
```

<br>

**抛出错误**

程序也可以主动抛出一个错误，让执行流程直接跳转到`catch`块。抛出错误使用`throw`语句。


<br/>
<br/>


## 错误传播

如果代码发生了错误，有没有被`try...catch`捕获，那么程序执行流程会跳转到哪呢？

如果在一个函数内部发生了错误，它自身没有捕获，错误就会被抛到外层调用函数，如果外层函数也没有捕获，该错误会一直沿着函数调用链向上抛出，直到被JavaScript引擎捕获，代码终止执行。


<br/>
<br/>


## 异步错误处理

编写JavaScript代码时，我们要时刻牢记，JavaScript引擎是一个事件驱动的执行引擎，代码总是以单线程执行，而回调函数的执行需要等到下一个满足条件的事件出现后，才会被执行。

所以，涉及到异步代码，无法在调用时捕获，原因就是在捕获的当时，回调函数并未执行。类似的，当我们处理一个事件时，在绑定事件的代码处，无法捕获事件处理函数的错误。















<br/>
<br/>

---

<br/>
<br/>






















# underscore

正如jQuery统一了不同浏览器之间的DOM操作的差异，让我们可以简单地对DOM进行操作，underscore则提供了一套完善的函数式编程的接口，让我们更方便地在JavaScript中实现函数式编程。

jQuery在加载时，会把自身绑定到唯一的全局变量`$`上，underscore与其类似，会把自身绑定到唯一的全局变量`_`上，这也是为啥它的名字叫underscore的原因。






























































<br/>
<br/>

---

<br/>
<br/>




















# Node.js

从本章开始，我们就正式开启JavaScript的后端开发之旅。

Node.js是目前非常火热的技术，但是它的诞生经历却很奇特。

Google认为要运行现代Web应用，浏览器必须有一个性能非常强劲的JavaScript引擎，于是Google自己开发了一个高性能JavaScript引擎，名字叫V8，以BSD许可证开源。

话说有个叫Ryan Dahl的歪果仁，他的工作是用C/C++写高性能Web服务。对于高性能，异步IO、事件驱动是基本原则，但是用C/C++写就太痛苦了。于是这位仁兄开始设想用高级语言开发Web服务。他评估了很多种高级语言，发现很多语言虽然同时提供了同步IO和异步IO，但是开发人员一旦用了同步IO，他们就再也懒得写异步IO了，所以，最终，Ryan瞄向了JavaScript。

因为JavaScript是单线程执行，根本不能进行同步IO操作，所以，JavaScript的这一缺陷导致了它只能使用异步IO。

选定了开发语言，还要有运行时引擎。这位仁兄曾考虑过自己写一个，不过明智地放弃了，因为V8就是开源的JavaScript引擎。让Google投资去优化V8，咱只负责改造一下拿来用，还不用付钱，这个买卖很划算。

于是在2009年，Ryan正式推出了基于JavaScript语言和V8引擎的开源Web服务器项目，命名为Node.js。虽然名字很土，但是，Node第一次把JavaScript带入到后端服务器开发，加上世界上已经有无数的JavaScript开发人员，所以Node一下子就火了起来。

在Node上运行的JavaScript相比其他后端开发语言有何优势？最大的优势是借助JavaScript天生的事件驱动机制加V8高性能引擎，使编写高性能Web服务轻而易举。

其次，JavaScript语言本身是完善的函数式语言，在前端开发时，开发人员往往写得比较随意，让人感觉JavaScript就是个“玩具语言”。但是，在Node环境下，通过模块化的JavaScript代码，加上函数式编程，并且无需考虑浏览器兼容性问题，直接使用最新的ECMAScript 6标准，可以完全满足工程上的需求。

<br>

Node.js是一个开源和跨平台的JavaScript runtime environment。



<br/>
<br/>


## 安装Node.js和npm

由于Node.js平台是在后端运行JavaScript代码，所以需要在本机按照Node环境。


<br/>
<br/>


### 安装Node.js

详情请看官网文档。


<br/>
<br/>


### npm

npm is the standard package manager for Node.js.

npm其实是Node.js的包管理工具。
为啥我们需要一个包管理工具呢？因为我们在Node.js上开发时，会用到很多别人写的JavaScript代码。如果我们要使用别人写的某个包，每次都根据名称搜索一下官方网站，下载代码，解压，再使用，非常繁琐。于是一个集中管理的工具应运而生：大家都把自己开发的模块打包后放到npm官网上，如果要使用，直接通过npm安装就可以直接用，不用管代码存在哪，应该从哪下载。

更重要的是，如果我们要使用模块A，而模块A又依赖于模块B，模块B又依赖于模块X和模块Y，npm可以根据依赖关系，把所有依赖的包都下载下来并管理起来。否则，靠我们自己手动管理，肯定既麻烦又容易出错。

讲了这么多，npm究竟在哪？其实npm已经在Node.js安装的时候顺带装好了。


<br/>
<br/>


## 第一个Node程序

在前面的章节中，编写的JavaScript代码都是在浏览器中运行的，因此，我们可以直接在浏览器中敲代码，然后直接运行。

从本章开始，我们编写的JavaScript代码将不能在浏览器环境中执行了，而是在Node环境中执行。

```js
'use strics';

console.log('Hello, world.')
```

执行:

```sh
node hello.js
# Hello, world.
```

<br>

**严格模式**

在服务器环境下，如果有很多JavaScript文件，每个文件都写上`'use strict';`很麻烦。我们可以给Nodejs传递一个参数，让Node直接为所有js文件开启严格模式：

```sh
node --use_strict
```



<br/>
<br/>
<br/>



## 模块

为了编写可维护的代码，我们把很多函数分组，分别放到不同的文件里，这样，每个文件包含的代码就相对较少，很多编程语言都采用这种组织代码的方式。在Node环境中，一个`.js`文件就称之为一个模块（module）。

使用模块有什么好处？最大的好处是大大提高了代码的可维护性。其次，编写代码不必从零开始。当一个模块编写完毕，就可以被其他地方引用。我们在编写程序的时候，也经常引用其他模块，包括Node内置的模块和来自第三方的模块。

```js
'use strict';

var s = 'Hello';

function greet(name) {
    console.log(s + ', ' + name + '!');
}

// 把函数greet作为模块的输出暴露出去，这样其他模块就可以使用greet函数了
module.exports = greet;
```

```js
// 使用模块
// 使用require引入模块，请注意路径
var greet = require('./hello');

var s = 'Michael';

greet(s);
```

<br>

**CommonJS规范**

这种模块加载机制被称为CommonJS规范。在这个规范下，每个`.js`文件都是一个模块，它们内部各自使用的变量名和函数名都互不冲突。

一个模块想要对外暴露变量（函数也是变量），可以用`module.exports = variable;`，一个模块要引用其他模块暴露的变量，用`var ref = require('module_name');`就拿到了引用模块的变量。



<br/>
<br/>
<br/>



## 基本模块









