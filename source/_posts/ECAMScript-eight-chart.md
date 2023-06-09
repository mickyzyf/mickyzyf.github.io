---
layout: post
title:  "ES6第八章 函数的扩展"
date:   2022-06-13 23:35:54
categories: ES6
tags: ES6
excerpt:	在ES6之前，不能直接为函数的参数指定默认值，只能采用变通的方法
mathjax: true
author:	朱羽飞
---

 

# 函数的扩展

## 函数参数的默认值

### 基本用法

在ES6之前，不能直接为函数的参数指定默认值，只能采用变通的方法

 function log(x,y){
  y = y|| 'World';
  console.log(x,y);
 }
 log('Hello'); //Hello World
 log('Hello','China') //Hello China
 log('Helllo','') //Hello World
上面代码检查函数log的参数有没有赋值，如果没有，则指定默认值World，这种写法的缺点在于，如果没有参数y赋值了，但是对应的布尔值为false，则赋值不起作用，就像上面的代码最后一行，参数y等于空字符，结构被改为默认值。

为了避免这个问题，通常需要先判断一下参数y是否被赋值，如果没有，再等于默认值

 if(typeof y === 'undefined'){
  y = 'World';
 }

ES6允许为函数的参数设置默认值，即直接写在参数定义的后面。

 function log(x,y = 'World'){
  console.log(x,y);
 }
 log('Hello');
 log('Hello','China') //Hello China
 log('Hello','') //Hello

ES6的写法比ES5简介许多，而且非常自然

除了简洁，ES6的写法还有两个好处：首先，阅读代码的人，可以立即意识到哪些参数是可以省略的，不用查看函数体或文档，其次，有利于将来的代码优化，即使未来的版本在对外接口中，彻底拿掉这个参数，也不会导致以前的代码无法运行。

参数变量的默认申明的，所以不能用let或const再次声明

 function foo(x = 5){
  let x =1;//error
  const x =2;//error
 }

### 与解构赋值默认值结合使用

参数默认值可以与结构赋值的默认值，结合起来使用

 function foo({x,y=5}){
  console.log(x,y);
 }
 foo({}); //undefined,5
 foo({x:1}); //1,5
 foo({x:1,y:2}) //1,2
 foo() //TypeError:Cannot read property 'x' of undefined

另一个对象的结构赋值默认的例子

 function fetch(url,{body = '',method = 'GET',headers ={} }){
  console.log(method);
 }

 fetch('<http://example.com',{}>)
 //"GET"

 fetch('<http://example.com>')
 //报错

比较下面两种写法的区别

 //写法一
 function m1({x = 0,y = 0} = {}){
  return [x,y];
 }

 //写法二
 function me({x,y} = {x:0,y:0}){
  return [x,y];
 }
写法一函数参数的默认值是空对象，但是设置了对象解构赋值的默认值

写法而函数参数的默认值是一个有具体属性的对象，但是没有设置对象解构赋值的默认值。

 //函数没有参数的情况
 m1()   //[0,0]
 m2()   //[0,0]

 //x和y都有值的情况
 m1({x:3,y:8})   //[3,8]
 m2({x:3,y:8})  //[3,8]

 //x有值，y无值的情况
 m1({x:3})    //[3,0]
 m2({x:3})    //[3,undefined]

 //x和y都无值的情况
 m1({})   //[0,0]
 me({})   //[undefined,undefined]

 m1({z:3})  //[0,0]
 me({z:3})  //[undefined,undefined]

### 参数默认值的位置

通常情况下，定义了默认值的参数，应该是函数的尾参数，因为这样比较容易看出来，到底省略了哪些参数。如果非尾部的参数设置默认值，实际上这个参数是没法省略的。

 //例一
 function f(x = 1,y){
  return [x,y];
 }

 f()  //[1,undefined]
 f(2) //[2,undefined]
 f(,1) //报错
 f(undefined,1)  //[1,1]

 //例二
 function f(x,y = 5,z){
  return [x,y,z];
 }
 f()  //[undefined,5,undefined]
 f(1)  //[1,5,undefined]
 f(1,2)  //报错
 f(1,undefined,2)  //[1,5,2]
这里有默认值的参数不是尾参数，这时，无法只省略该参数，而不是省略它后面的参数，除非显式输入undefined。

如果传入undefined，将触发参数等于默认值，null则没有这个效果

 function foo(x=5,y=6){
  console.log(x,y);
 }

 foo(undefined,null)
 //5 null

x参数对用undefined,结果触发了默认值，y参数等于null，就没有触发默认值。

### 函数的length属性

指定默认值以后，函数的length属性，将返回没有指定默认值的参数个数。也就是说，指定了默认值以后，length属性将失真。

 (function (a){}).length //1
 (function (a = 5){}).length //0
 (function (a,b,c = 5){}).length //2

length属性的返回值，等于函数的参数个数减去指定了默认值的参数个数。其中有一个参数c指定了默认值，因此length属性等于3减去1，最后得到2。

这是因为length属性的含义是，该函数预期传入的参数个数。某个参数指定默认值以后，预期传入的参数个数就不包括这个参数了。

 (function(...args) {}).length //0

如果设置了默认值的参数不是尾参数，那么length属性也不再计入后面的参数了

 (function (a = 0,b,c){}).length //0
 (function (a,b = 1,c){}).length //1

### 作用域

需要注意的地方是，如果参数默认值是一个变量，则该变量所处的作用域，与其他变量的作用域规则是一样的，即先是当前函数的作用域，然后才是全局作用域。

 var x = 1;
 function f(x,y = x){
  console.log(y);
 }
 f(2)  //2
上面代码中，参数y的默认值等于x。调用时，由于函数作用域内部的变量x已经生成，所以y等于参数x，而不是全局变量x。如果调用时，函数作用域内部的变量x没有生成，结果就会不一样。

 let x  = 1;

 function f(y = x){
  let x =2;
  console.log(y)
 }
 f() //1
函数调用时，y的默认值变量x尚未在函数内部生成，所以x指向全局变量

 function f(y = x){
  let x =2;
  console.log(y);
 }
 f() //ReferenceError : x is not defined

这样写也会报错
 var x =1;
 function foo(x = x){
  //...
 }
 foo() //ReferenceError : x is not defined
函数foo的参数x的默认值也是x。这时，默认值x的作用域是函数作用域，而不是全局作用域。由于在函数作用域中，存在变量x，但是默认值在x赋值之前就执行了，所以这时属于暂时性死区，任何对x的报错都会报错。

 let foo = 'outer';
 function bar(func = x => foo){
  ley foo =  "inner";
  console.log(func()); //outer
 }
 bar();
函数bar的参数func的默认值是一个匿名函数，返回值为变量foo。这个匿名函数声明时，bar函数的作用域还没有形成，所以匿名函数里面的foo指向外层作用域的foo，输出outer。

 function bar(func =() => foo){
  let foo = 'inner';
  console.log(func());
 }

 bar() //ReferenceError: foo is not defined
匿名函数里面的foo指向函数外层，但是函数外层并没有声明foo，所以就报错了。

这有一个更复杂的例子

 var x = 1;
 function foo (x,y = function(){ x = 2;}){
  var x = 3;
  y();
  console.log(x);
 }
 foo() //3
函数foo的参数y的默认值是一个匿名函数，函数foo调用时，它的参数x的值为undefined，所以y函数内部一开始是undefined，后来被重新赋值为2.但是，函数foo内部重新声明了一个x，值为3，这两个x是不一样的，互相不产生影响，因此最后输出3。

如果将var x = 3的var去掉，两个x就是一样的了，最后输出就是2。

 var x =1 ;
 function foo(x,y = function(){x =2;}){
  x = 3;
  y();
  console.log(x);
 }
 foo() //2

### 应用

利用参数默认值，可以指定某一个参数不得省略，如果省略就抛出一个错误。

 function throwIfMissing(){
  throw new Error('Missing parameter');
 }
 function foo(mustBeProvided = throwIfMissing()){
  return mustBeProvided;
 }
 foo();
 //Error: Missing parameter

上面代码的foo函数，如果调用的时候没有参数，就会调用默认值throwIfMissing函数，从而抛出错误。

## rest参数

ES6引入rest参数(形式为"...变量名")，用于获取函数的多余参数，这样就不需要使用argument对象了。rest参数搭配的变量是数组，该变量将多余的参数放入数组中。

 function add(...values){
  let sum = 0;
  
  for(var val of values){
   sum += val;
  }
  
  return sum;
 }
 add(2,5,3) //10
上面代码的add函数是一个求和函数，利用rest参数，可以向该函数传入任意数目的参数

下面是一个rest参数替代arguments变量的例子

 //arguments变量的写法
 function sortNumbers(){
  return Array.prototype.slice.call(arguments).sort();
 }

 //rest参数的写法
 const sortNumbers = {...numbers} => numbers.sort();
可以发现rest参数的写法更加自然也更加简洁

rest参数中的变量代表一个数组，所以数组特有的方法都可以用于这个变量。下面是一个利用rest参数改写数组push的例子。

 function push(array, ...items){
  items.forEach(function(item){
   array.push(item);
   console.log(item);
  });
 }

 var a = [];
 push(a,1,2,3);
注意，rest参数之后不能再有其他参数(即只能是最后一个参数)，否则会报错。

 //报错
 function f(a, ...b,c){
  //...
 }
函数的length属性，不包含rest参数

 (function(a){}).length //1
 (function(...a){}).length //0
 (function(a, ...b){}).length //1

## 扩展运算符

### 含义

扩展运算符(spread)是三个点(...)。它好比rest参数的逆运算，将一个数组转为用逗号分隔的参数序列。

 console.log(...[1,2,3])
 //1 2 3

 console.log(1,...[2,3,4],5)
 //1 2 3 4 5

 [...document.querySelectorAll('div')]
 //[<div>,<div>,<div>]

该运算只要用于函数调用。

 function push(array,...items){
  array.push(...items);
 }

 function add(x,y){
  return x+y;
 }

 var numbers = [4,38];
 add(...numbers) //42
array.push(...items)和add(...numbers)都是函数的调用，都使用了扩展运算符将一个数组变为参数序列

扩展运算符，可以结合正常函数参数使用，非常灵活

### 替代数组的apply方法

由于扩展运算符可以展开数组，所以不再需要啊apply方法，将数组转为函数的参数了。

 //ES5的写法
 function f(x,y,z){
  //...
 }
 var args = [0,1,2];
 f.apply(null,args);

 //ES6的写法
 function f(x,y,z){
  // ...
 }

 var args = [0,1,2];
 f(...args);

下面是扩展运算符取代apply方法的一个实际的例子，应用Math.max方法，简化求出一个数组最大元素的写法。
 //ES5的写法
 Math.max.apply(null,[14,3,77])

 //ES6的写法
 Math.max(...[14,3,77])

 //等同于
 Math.max(14,3,77)
JavaScript不提供求数组最大元素的函数，所以只能套用Math.max函数，将数组转为一个参数序列，然后求最大值，有了扩展运算符以后，就可以只直接使用Math.max了。

另一个例子是通过push函数，将一个数组添加到另一个数组的尾部。

  //ES5的写法
 var arr1 = [0,1,2];
 var arr2 = [3,4,5];
 Array.prototype.push.apply(arr1,arr2);

 //ES6的写法
 var arr1 = [0,1,2];
 var arr2 = [3,4,5];
 arr1.push(...arr2);
push方法的参数不能是数组，所以只好通过applay方法变通使用push。有了扩展运算符，就可以直接将数组传入push方法。

### 扩展运算符的应用

1. 合并数组

 扩展运算符提供了数组合并的新写法。
  
  //ES5
  [1,2].concat(more)
  //ES6
  [1,2,...more]
  
  var arr1 = ['a','b'];
  var arr2 = ['c'];
  var arr3 = ['d','e'];

  //ES5的合并数组
  arr1.concat(arr2,arr3);
  //['a','b','c','d','e']
2. 与解构赋值结合
  
  const[first,...rest] = [1,2,3,4,5];
  //first //1
  //rest  //[2,3,4,5]
  
  const[first,...rest] = [];
  first //undefined
  rest //[];

  const [first,...rest] = ["foo"];
  //first //"foo"
  // rest //[]
 如果扩展运算符用于数组赋值，只能放在参数的最后一位，否则会报错。
  
  const [...butLat,last] = [1,2,3,4,5];
  //报错
  const [first,...middle,last] = [1,2,3,4,5];
  //报错

3. 函数的返回值

 JavaScript的函数只能返回一个值，如果需要返回多个值，只能返回数组或对象。扩展运算符提供了解决这个问题的一种变通方法。

  var dateFields = readDateFields(database);
  var d = new Date(...dateFields);
 上面代码从数据库取出一行数据，通过扩展运算符，直接将其传入构造函数Date

4. 字符串

 扩展运算符还可以将字符串转为真正的数组。
  
  [...'hello']
  //["h","e","l","l","o"]
 这样写，有一个好处就是，能正确识别32位的Unicode字符
  
  'x\uD83D\uDE89y'.length //4
  [...'x\uD83D\uDE89y'].length //3
 JavaScript会将32位Unicode字符，识别为2个字符，采用扩展运算符就没有这个问题，因此，正确返回字符串长度的函数，可以这样写

  function length(str){
   return [...str].length;
  }
  length('x\uD83D\uDE80y') //3
 凡是涉及操作32位Unicode字符的函数，都有这个问题，因此，最好都用扩展运算符改写。

 let str = 'x\uD83D\uDE80y';

 str.split('').reverse().join('');
 //'y\uDE80\uD83Dx'

 [...str].reverse().join('')
 //'y\uDE80\uD83Dx'
5. 实现Iterator接口的对象

 任何Iterator接口的对象，都可以用扩展运算符转为真正的数组。

  var nodeList = document.querySelectorAll('div');
  var array = [...nodeList];
 querySelectorAll方法返回的是一个nodeList对象。它不是数组，而是一个类似数组的对象。这时，扩展运算符可以将其转为真正的数组，原因就在于NodeList对象实现了Iterator接口

 对于那些没有部署Iterator接口的类似数组对象，扩展运算符就无法将其转为真正的数组。

  let arrayLike = {
   '0':'a',
   '1':'b',
   '2':'c',
   length:3
  };
  //TypeError:Cannot spread non-iterable object.
 let arr = [...arrayLike];

 上面代码中，arrayLike是一个类似数组的对象，但是没有部署Iterator接口，扩展运算符就会报错，这时，可以改为使用Array.from方法将arrayLike转为正真的数组。
6. Map和Set结构，Generator函数
 扩展运算符内部调用的是数据结构的Iterator接口，因此只要具有Iterator接口的对象，都可以使用扩展运算符，比如Map结构。

  let map = new Map([
   [1,'one'],
   [2,'two'],
   [3,'three'],
  ])
  let arr = [...map.keys()];//[1,2,3]
 Generator函数运行后，返回一个遍历器对象，因此也可以使用扩展运算符。

  var go = function(){
   yield 1 ;
   yield 2;
   yield 3;
  };
  [...go()]//[1,2,3]
 变量go是一个Generator函数，执行后返回的是一个遍历器对象，对这个遍历器对象执行扩展运算符，就会将内部遍历得到的值，转为一个数组。

 如果对没有iterator接口的对象，使用扩展运算符，将会报错。

 var obj = {a:1,b:2};
 let arr = [...obj]; //TypeError: Cannot spread non-iterable object

## name属性

函数的name属性，返回该函数的函数名。

 function foo(){}
 fo.name //"foo"
这个属性早就被浏览器广泛支持，但是直到es6，才将其写入标准。

需要注意的是，ES6对这个属性的行为作出了一些修改。如果将一个匿名函数赋值给一个变量，ES5的name属性，会返回空字符串，而ES6的name属性会返回实际的函数名。

 var func1 = function (){}

 //ES5
 func1.name //""

 //ES6
 func1.name //"func1"
func1等于一个匿名函数，ES5和ES6的name属性返回值不一样。
如果将一个具名函数赋值给一个变量，则ES5和ES6的name属性都返回这个具名函数原本的名字。

 const bar = function baz(){}

 //ES5
 bar.name //"baz"

 //ES6
 bar.name //"baz"
Function构造函数返回的函数实例，name属性值为"anonymous"。

 (new Function).name //"anonymous"
binde返回的函数，name属性值会加上"bound"前缀。

 function foo(){}
 foo.bind({}).name //"bound foo"

 (function(){}).bind({}).name  //"bound"

## 箭头函数

### 基本用法

ES6允许使用"箭头"(=>)定义函数。

 var f = v => v;
上面的箭头函数等同于；

 var f = function (v){
  return v;
 }

如果箭头函数不需要参数或需要多个参数，就是用一个圆括号代表参数部分。

 var f = () => 5;
 //等同于
 var f = function () {return 5};

 var sum = (num1,num2) => num1 + num2;
 //等同于
 var sum = function(num1,num2){
  return num1 + num2;
 }
如果箭头函数的代码块部分多于一条语句，就要使用大括号将它们括起来，并且使用return语句返回

 var sum = (num1,num2) => {return num1 + num2;}
由于大括号被解释为代码块，所以如果箭头函数直接返回一个对象，必须在对象外面加上括号。

 var getTempItem = id => ({id:id,name:"Temp"});
箭头函数可以与变量解构结合使用。

 const full = ({first,last}) => first + ' ' +last;

 //等同于
 function full(person){
  return person.first + '' +person.last;
 }
箭头函数使得表达更加简洁

 const isEven = n => n%2 == 0;
 const square = n => n*n;
上面代码只用了两行，就定义了两个简单的工具函数。如果不用箭头函数，可能就要占用多行，而且还不如现在这样写醒目。

箭头函数的一个用处是简化回调函数。

 //正常函数写法
 [1,2,3].map(function (x){
  return x * x;
 });
 //箭头函数写法
 [1,2,3].map(x => x*x);

 //正常函数写法
 var result = values.sort(function (a,b){
  return a - b;
 });
 //箭头函数写法
 var result = values.sort((a,b) => a-b);
同样，rest参数与箭头函数也可以混合使用

 const numbers = (...nums) => nums;

 numbers(1,2,3,4,5)
 //[1,2,3,4,5]

 const headAndTail = (head, ...tail) => [head,taill];

 headAndTail(1,2,3,4,5)
 //[1,[2,3,4,5]]

### 箭头函数使用注意点

1. 函数体内的this对象，就是定义时所在的对象，而不是使用时所在的对象。
2. 不可以当作构造函数，也就是说，不可以使用new命令，否则会抛出一个错误。
3. 不可以使用arguments对象，该对象在函数体中不存在，如果要用，使用Rest参数代替
4. 不可以使用yield命令，因此箭头函数不能用作Generator函数

第一点尤其值得注意，this对象的指向是可变的，但是在箭头函数内，它是固定的

 function foo(){
  setTimeout(() => {
   console.log('id',this.id);
  },100);
 }
 var id = 21;

 foo.call({id:42});
 //id:42
setTimeout的参数是一个箭头函数，这个箭头函数的定义生效在foo函数生成时，而它的真正执行要等到100毫秒后。如果是普通函数，执行时this应该指向全局对象window，这是应该输出21.但是，箭头导致this总是指向函数定义生效时所在的对象，所以输出的是42。

箭头函数可以让setTimeout里面的this，绑定定义时所在作用域，而不是指向运行时坐在的作用域。

 function Timer(){
  this.s1 =0;
  this.s2 =0;
  //箭头函数
  setInterval(() => this.s1++,1000);
  
  //普通函数
  setInterval(function(){
   this.s2++;
  },1000);
 }

 var timer new Timer();

 setTimeout(() => console.log('s1:',timer.s1),3100);
 setTimeout(() => console.log('s2:',timer.s2),3100);
 //s1:3
 //s2:0
Timer函数内部设置了两个定时器，分别使用了箭头函数和普通函数。前者的this绑定定义时所在的作用域(即Timer函数)，后者的this指向运行时所在的作用域(全局对象)。所以，3100毫秒之后，timer.s1被更新了三次，而timer.s2一次都没有更新。

箭头函数可以让this指向固定化，这种特性很有利于封装回调函数。下面是一个例子，DOM事件的回调函数封装在一个对象里面。

 var handler = {
  id:'123456',

  init:function(){
   document.addEventListener('click',
   event => this.doSomething(event.type),false);
  },

  doSomething: function(type){
   console.log('Handling' + type + 'for' + this.id);
  }
 };
init方法中使用了箭头函数，这导致这个箭头函数里面的this，总是指向handler对象。否则，回调函数运行时，this.doSomething这一行会报错，因为此时this指向document对象。

this指向的固定化，并不是因为箭头函数内部有绑定this的机制，实际原因是箭头函数根本没有自己的this，导致内部的this就是外层代码块的this。正因为它没有this，所以也就不能用作构造函数。

所以，箭头函数转成ES5的代码如下

 //ES6
 function foo(){
  setTimeout(() => {
   console.log('id:',this.id);
  },100);
 };

 //ES5
 function foo(){
  var _this = this;

  setTimeout(function (){
   console.log("id:",_this.id);
  },100);
 };
请问下面的代码之中有几个this?

 function foo(){
  return () =>{
   return () => {
    return () =>{
     console.log('id:',this.id);
    };
   };
  };
 }
 var f = foo.call({id:1});

 var t1 = f.call({id:2})()();  //id:1
 var t2 = f().call({id:3})(); //id:1
 var t3 = f()().call({id:4}); //id: 1
上面代码，只有一个this，就是函数foo的this，所以t1、t2、t3都输出同样的结果，因为所有内层函数都是箭头函数，都没有自己的this，它们的this都是最外层foo函数的this。

除了this，以下三个变量在箭头函数之中也是不存在的，指向外层函数的对应变量：arguments、super、new.target。

 function foo(){
  setTimeout(() =>{
   console.log('args:',arguments);
  },100);
 }

 foo(2,4,6,8)
 //args:[2,4,6,8]
上面代码中，箭头函数内部的变量arguments，其实是函数foo的arguments变量。

另外，由于箭头函数没有自己的this，所以当然也就不能用call()、apply()、bind()这些方法改变this的指向

 (function(){
  return[
   (() => this.x).bind({x:'inner'})()
  ];
 }).call({x:'outer'});
 //['outer']
上面代码中，箭头函数没有自己的this，所以bind方法无效，内部的this指向外部的this。
长期以来，JavaScript语言的this对象一直是一个令人头痛的问题，在对象方法中使用this，必须非常小心。箭头函数"绑定"this,很大程度上解决了这个困扰。

### 嵌套的箭头函数

箭头函数内部，还可以再使用箭头函数。下面是一个ES5语法的多重嵌套函数。

 function ionsert(value){
  return {into:function (array){
   return {after: function (afterValue){
    array.splice(array.indexOf(afterValue) + 1,0,value);
    return array;
   }};
  }};
 }
 insert(2).into([1,3]).after.(1);//[1,2,3]
上面这个函数，可以使用箭头函数改写。

 let insert = (value) => ({into:(array) => ({after:(afterValue) =>{
   array.splice(array.indexOf(afterValue) + 1, 0,value);
   return array;
  }})
 });
 insert(2).into([1,3]).after(1);//[1,2,3];

下面是一个部署管道机制(pipeline)的例子，即前一个函数的输出是后一个函数的输入。

 const pipeline =(...funcs) =>
  varl => funcs.reduce((a,b) => b(a),val);
 const plus1 = a => a + 1;
 const mult2 = a => a * 2;
 const addThenMult = pipeline(plus1,mult2);

 addThenMult(5);
 //12
如果觉得上面的写法可读性比较差，也可以采用下面的写法。

 const plus1 = a => a +1;
 const mult2 = a => a *2;

 mult2(plus1(5))
 //12

## 函数绑定

箭头函数可以绑定this对象，大大减少了显示绑定this对象的写法(call、apply、bind)。但是，箭头函数并不适用于所有场合，所以ES7提出了"函数绑定"(function bind)运算符，用来取代call、apply、bind调用。虽然该语法还是ES7的一个提案，但是Babel转码器已经支持。

函数绑定运算符是并排的两个双冒号(::),双冒号左边是一个对象，右边是一个函数。该运算会自动将左边的对象，作为上下文环境(即this对象)，绑定到右边的函数上面。

 foo::bar;
 //等同于
 bar.bind(foo);

 foo::bar(...arguments);
 //等同于
 bar.apply(foo,arguments);

 const hasOwnProperty = Object.prototype.hasOwnProperty;
 function hasOwn(obj,key){
  return obj::hasOwnProperty(key);
 }
如果双冒号左边为空，右边是一个对象方法，则等于将该对象绑定在该对象上面。

 var method = obj :: obj.foo;
 //等同于

 var method = ::obj.foo;

 let log = :: console.log;
 //等同于
 var log = console.log.bind(console);
由于双冒号运算符返回的还是原对象，因此，可以采用链式写法。

 //例一
 import {map,takeWhile,forEach } form "iterlib";

 getPlayers()

 ::map(x => x.character())
 ::takeWhile(x => x.strength >100)
 ::forEach(x => console.log(x));

 //例二
 let {find,html} = jake;

 document.querySelectorAll("div.myClass");
 ::find("p")
 ::html("hahaha");

## 尾调用优化

### 什么是尾调用？

尾调用(Tail Call) 是函数式编程的一个重要概念，本身非常简单，一句话就能说清楚，就是指某一个函数的最后一步是调用另一个函数。

 function f(x){
  return g(x);
 }
函数f的最后一步是调用函数g，这就叫尾调用。

 //情况一
 function f(x){
  let y = g(x);
  return y;
 }
 //情况二
 function f(x){
  return g(x) +1;
 }
 //情况三
 function f(x){
  g(x);
 }
上面这三种情况都不是尾调用，情况一是电泳函数g之后，还有赋值操作，所以不属于尾调用。情况二属于调用后还有操作。
情况三同
  
 function f(x){
  g(x);
  return undefined;
 }
所以也不是尾调用。

尾调用不一定出现在函数尾部，只要是最后一步操作即可。

 function f(x){
  if (x > 0){
   return m(x)
  }
  return n(x);
 }
函数m和n都属于尾调用，因为它们都是函数f的最后一步操作

### 尾调用优化

尾调用之所以与其他调用不同，就在于它的特殊的调用位置。

我们知道，函数调用会在内存形成一个"调用记录"，又称"调用帧"(call frame),保存调用位置和内部变量等信息。如果在函数A的内部调用函数B，那么在A的调用帧上方，还会形成一个B的调用帧。等到B运行结束，将结果返回到A，B的调用帧才会消失。如果函数B内部还调用函数C，那就还有一个C的调用帧，以此类推。所有的调用帧，就形成一个"调用栈"(call stack)。

尾调用由于是函数的最后一步操作，所以不需要保留外层函数的调用帧，因为调用位置、内部变量等信息都不会再用到了，只要直接用内层函数的调用帧，取代外层函数的调用帧就可以了。

 function f(){
  let m =1;
  let n =2;
  return g(m + n);
 }
 f();

 //等同于
 function f(){
  return g(3);
 }
 f();

 //等同于
 g(3);
上面代码中，如果函数g不是尾调用，函数f就需要保存内部变量m和n的值、g的调用位置等信息。但由于调用g之后，函数f就结束了，所以执行到最后一步，完全可以删除f(x)的调用帧。只保留g(3)的调用帧。

这就叫做"尾调用优化"(Tail call optimization)，即只保留内层函数的调用帧。如果所有函数都是尾调用，那么完全可以做到每次执行时，调用帧只有一项，这将大大节省内存。这既是"尾调用优化"的意义

注意：只有不再用到外层函数的内部变量，内层函数的调用帧才会取代外层函数的调用帧，否则就无法进行"尾调用优化"。

 function addOne(a){
  var one =1;
  function inner(b){
   return b + one;
  }
  return inner(a);
 }
上面的函数不会进行尾调用优化，因为内层函数(inner)用到了外层addOne的内部变量one。

### 尾递归

函数调用自身，称为递归。如果尾调用自身，就称为尾递归。
递归非常耗内存，因为需要同时保存成千上万个调用帧，很容易发生"栈溢出"错误(stack overflow)。但对于尾递归来说，由于只存在一个调用帧，所以永远不会发生"栈溢出"错误

 functioon factorial(n){
  if(n ===1) return 1;
  return n * factorial(n - 1);
 }
 factorial(5) //120
上面代码是一个阶乘函数，计算n的阶乘，最多需要保存n个调用记录，复杂度O(n)。

如果改成尾递归，只保留一个调用记录，复杂度为O(1)。

 function factorial(n,total){
  if (n === 1) return total;
  return factorial(n -1 ,n * tatal);
 }
 factorial(5,1) //120

还有一个比较著名的例子，就是计算fibonacci数列，也能充分说明尾递归优化的重要性，如果是非尾递归的fibonacci递归方法

 function Fibonacci (n){
  if (n <= 1){
   return 1;
  }
  return Fibonacci(n -1) +Fibonacci(n -2);
 }
 Fibonacci(10);//  89
 //Fibonacci(100)
 //Finbonacc(500)
 //堆栈溢出了
如果我们使用尾递归优化过的fibonacci递归算法

 function Finbonacci2(n ,ac1 =1,ac2 = 1){
  if(n <= 1){ return ac2};

  return Fibonacci2(n-1,ac2,ac1 + ac2);
 }
 Fibonacci2(100) //573147844013817200000
 Fibonacci2(1000) //7.0330367711422765e+208
 Fibonacci2(10000) //Infinity
由此可见，"尾调用优化"对递归操作意义重大，所以一些函数式编程与雅安将其写入语言规格。ES6也是如此，所有ECMAScript的实现，都必须部署"尾调用优化"。这就是说，ES6中，只要使用尾递归，就不会发生栈溢出，相对节省内存。

### 递归函数的改写

尾递归的实现，往往需要改写递归函数，确保最后一步只调用自身。做到这一点的方法，就是把所有用到的内部变量改写成函数的参数。比如上面的例子，阶乘函数factorial需要用到一个中间变量total，那就把这个中间变量改写成函数的参数。这样做的缺点就是不太直观，第一眼很难看出来，为什么计算5的阶乘，需要传入两个参数5和1？

 function tailFactorial(n,total){
  if (n === 1)
   return total;
  return tailFactorial(n-1,n *total);
 }

   function factorial(n){
  return tailFactorial(n,1);
 }
 factorial(5); //120
上面代码通过一个正常形式的阶乘函数factorial,调用尾递归函数tailFactorial，看起来就正常多了。

函数式编程有一个概念，叫做柯里化(currying),意思是将多参数的函数转化成单参数的形式。这里也可以使用柯里化。

 function currying(fn,n){
  return function (m){
   return fn.call(this,m,n);
  }
 }

 function tailFactorial(n,total){
  if (n === 1) return total;
  return tailFactorial(n-1,n*total);
 }

 const factorial = currying(tailFactor,1);

 factorial(5) //120
上面代码通过柯里化，将尾递归函数tailFactorial变为只接受1个参数的factorial。

第二种方法就简单多了，就是采用ES6的函数默认值

 function factorial(n,total = 1){
  if (n === 1)
   return total;
  return factorial(n-1,n * total);
 }
 factorial(5) //120
上面代码中，参数total有默认值1，所以调用时不用提供这个值。

总结一下，递归本质上是一个循环操作。纯粹的函数式编程语言没有循环操作命令，所有的循环都用递归实现，这就是为什么尾递归对这些语言极其重要。

### 严格模式

ES6的尾调用优化只在严格模式下开启，正常模式是无效的

这是因为在正常模式下，函数内部有两个变量，可以跟踪函数的调用栈。

* func.arguments:返回调用时函数的参数
* func.caller：返回调用当前函数 的那个函数

尾调用优化发生时，函数调用栈会改写，因此上面两个变量会失真。严格模式禁用这两个变量，所以尾调用模式仅在严格模式下生效

 function restricted(){
  "use strict";
  restructed.caller;//报错
  restricted.arguments;//报错
 }
 restricted();

## 函数参数的尾逗号

ES7有一个提案，允许函数的最后一个参数有尾逗号

目前，函数定义和调用时，都不允许有参数的尾逗号

 function clownsEverywhere(param1,param2){
  //...
 }
 clownsEverywhere('foo','bar');
如果以后要遭函数的定义之中添加参数，就势必还要添加一个逗号，这对版本管理系统来说，就会显示，添加逗号的哪一行也发生了变动。这看上去有点冗余，因此，新提案允许定义和调用时，尾部直接有一个逗号。

 function clownsEverywhere(param1,param2,){}

 clownsEverywhere('foo','bar',);
