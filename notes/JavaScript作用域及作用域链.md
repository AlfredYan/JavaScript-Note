# JavaScript作用域及闭包

## 1.编译及变量查找方法

**分词/词法分析：** 将有意义的字符串分解成有意义的代码块（词法单元）。例如：var a = 2; 会被分解成：var、a、=、2、;。空格是否被当做词法单元，取决于空格在这门语言中是否有意义。

**解析/语法分析：** 将词法单元流转换成“抽象语法树”（AST）。

**代码生成：** 将AST转换为可 执行代码。

对于JavaScript来说，编译发生在代码执行前的几微秒。

**LHS：** 赋值操作的左侧。寻找变量的容器本身，从而可以对其**赋值**，不成功的LHS会（在非严格模式下）自动隐式地创建一个全局变量。（var a = 2;）

**RHS：** 赋值操作的非左侧。查找某个变量的**值**。不成功的RHS会抛出ReferenceError。（console.log(a)）

*注：ReferenceError同作用域判别失败有关；TypeError表示作用判别成功，但对结果的操作是非法的。*

**变量的赋值操作：** 首先编译器会在当前作用域中声明一个变量（如果之前没有声明过），然后在运行时引擎会在作用域中进行LHS查找该变量，如果找到则对它赋值。

## 2.词法作用域

**作用域：** 作用域是一套规则，用于确定在何处以及如何查找变量。可分为**词法作用域**和**动态作用域** 。

**词法作用域：** 定义过程发生**在代码的书写阶段**。由写代码时将变量和块作用域写的位置决定。JavaScript采用的是词法作用域。

```javascript
function foo(){
  console.log(a);
}

function bar(){
  var a = 3;
  foo();
}

var a = 2;

bar(); // 2
```

**动态作用域：** 作用域在运行时决定，决定于何处调用，作用域链基于调用栈。

**JavaScript 作用域查找：** 作用域查找会在找到第一个匹配的标识符时停止。在多层嵌套的定义域中，如果有同名的标识符，则会发生“遮蔽效应”。查找始终从运行时所处的最内部作用开始，逐级向上进行，直到遇到第一个匹配的标识符。

*注：全局变量会自动成为全局对象的属性，因此可以通过对全局对象属性的引用来进行访问（window.a）。由此可以访问被同名变量所遮蔽的全局变量。* 

## 3.函数作用域 

**函数作用域：** 属于这个函数的全部变量都可以在整个函数的范围内使用及复用（嵌套的作用域中也可以使用）。

**函数表达式与函数声明：** 如果function是声明中的第一个词，那么就是一个函数声明，否则就是一个函数表达式。

**包装函数的申明：** 以(function...开始，函数会被当做一个函数表达式。可以不需要函数名并自动运行。将内部变量隐藏，不污染外部作用域。

```javascript
var a = 2;

//立即执行函数表达式（IIFE）
//通过在末尾加上一个()来立即执行函数
//当做函数调用并传递参数进去
(function(global){
  var a = 3;
  console.log(a); // 3
  console.log(global.a); // 2
})(window);

console.log(a); // 2
```

**匿名与具名：** 函数表达式可以匿名，但声明不可以省略函数名。 

```javascript
// 匿名函数表达式
setTimeout( function() {
  console.log("I waited 1 second!")
}, 1000 )
```

**匿名函数的表达式的缺点：** 

1. 匿名函数在栈追踪中不会显示出有意义的函数名，调试困难。
2. 难以引用自身。
3. 由于没有函数名，降低了代码的可读性与可理解性。

## 4.提升

**提升规则：** 

1. 声明本省会被提升，而赋值或其他运行逻辑会留在原地。

   ```javascript
   console.log(a); // undefined
   var a = 2;

   //实际代码执行顺序
   var a;
   console.log(a); // undefined
   a = 2;
   ```

2. 定义声明在编译阶段进行（var a;)，赋值声明会被**留在原地**等待执行阶段（a = 2;）。

3. 函数表达式不会被提升，即使是具名的函数表达式，名称标识符在赋值前也无法在作用域中使用。

   ```javascript
   foo(); // TypeError
   bar(); // ReferenceError
   var foo = function bar(){
     console.log("函数表达式");
   }
   ```

4. 每个作用域都会进行提升操作。

5. 函数声明会首先被提升，然后才是变量，重复的var声明会被忽略，但出现在后面的同名函数声明还是可以覆盖前面的。

   ```javascript
   foo(); // 3
   var foo;

   function foo(){
     console.log(1);
   }

   foo = function(){
     console.log(2);
   }

   function foo(){
     console.log(3);
   }
   ```

   ​

6. 变量只能通过var关键字声明。

   ```javascript
   console.log(a); // undefined
   console.log(b); // b is not defined

   b = 1; // 在非严格模式下，给全局对象创建了一个新属性，但它不是变量
   var a = 2;
   ```

## 5.执行上下文栈

每当执行一段可执行代码时，即会进入一个执行上下文（EC）。执行上下文是一个抽象概念，用于与可执行代码概念进行区分。

定义执行上下文栈是一个数组

```javascript
ECStack = [];
```

**全局代码：** 在“程序”级处理的。不包括任何function体内代码。

```javascript
//在初始化阶段
ECStack = [
  globalContext
];
```

**函数代码：** 进入function函数代码时，ECStack被压入新元素。具体的函数代码不包括内部函数。

```javascript
(function foo(bar){
  if(bar){
    return;
  }
  foo(true);
})();

//第一次调用foo
ECStack = [
  <foo> functionContext,
  globalContext
];

//foo的递归调用
ECStack = [
  <foo> functionContext(recursion),
  <foo> functionContext,
  globalContext
]; 
```

每次return时，都会退出当前的执行上下文，相应的ECStack就会弹出，栈指针会自动移动位置。相关代码执行完成后，ECStack只会包含全局上下文，直到整个应用程序结束。

## 6.变量对象

变量对象（VO）是一个与执行上下文相关的特殊对象，它存储着上下文中声明的：

- 变量（var，变量声明）

- 函数声明（FunctionDeclaration，FD）

- 函数的形参

在具体实现层面变量对象只是一个抽象概念。

使用普通ECMAScript对象来表示变量对象：

```javascript
VO = {};
```

VO是执行上下文的一个属性

```javascript
activeExecutionContext = {
  VO: {
    //var、FD、function arguments
  }
}
```

例如：

```javascript
var a = 1;

function test(x){
  var b = 2;
}

test(3);

//全局上下文的变量对象
VO(globalContext) = {
  a: 1,
  test: <reference to function>
};

//test函数上下文的变量对象
VO(test functionContext) = {
  x: 3,
  b: 2
};
```

**全局上下文的变量对象：** 在进入任何执行上下文前就已经创建的对象；该对象只存在一份，它的属性在程序的任何地方都可以访问，全局对象的生命周期在程序退出时终止。

```javascript
global = {
  String: <...>,
  ...
  window: global
};

//变量对象就是全局对象自己
VO(globalContext) === global;

//由此才能间接通过全局对象的属性来访问它
var a = "test";

alert(a) // 直接访问，在VO(globalContext)中找到"test"

alert(window.a) // 间接通过global访问
```

**函数上下文中的变量对象：** 在函数执行上下文中，VO是不能直接访问的，此时由活动对象（AO）扮演VO角色。活动对象在进入函数上下文时被创建的，通过函数的arguments属性初始化。arguments属性的值是Arguments对象。

```javascript
VO(functionContext) === AO;

AO = {
  arguments: <ArgO>
};
```

**Arguments对象包括：** 

1. callee --- 指向当前函数的应用
2. length --- 真正传递的参数个数
3. properties-indexes（字符串类型的整数），其值是函数的参数值。

**处理上下文代码的2个阶段：** 

1. 进入执行上下文

   在进入执行上下文前，VO中已包含：函数的所有形参(如果在函数的执行上下文中)，函数声明以及变量声明。



