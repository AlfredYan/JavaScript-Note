# JavaScript作用域及闭包

## 1.编译及变量查找方法

**分词/词法分析：** 将有意义的字符串分解成有意义的代码块（词法单元）。例如：var a = 2; 会被分解成：var、a、=、2、;。空格是否被当做词法单元，取决于空格在这门语言中是否有意义。

**解析/语法分析：** 将词法单元流转换成“抽象语法树”（AST）。

**代码生成：** 将AST转换为可 执行代码。

对于JavaScript来说，编译发生在代码执行前的几微秒。

**LHS：** 赋值操作的左侧。寻找变量的容器本身，从而可以对其**赋值**，不成功的LHS会（在非严格模式下）自动隐式地创建一个全局变量。（a = 2;）

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
   //例子1
   alert(x); // function x(){}

   var x = 10;
   alert(x); // 10

   x = 20
   function x(){};

   alert(x); // 20
   ```

   ```javascript
   //例子2
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

6. 变量只能通过var关键字声明。

   ```javascript
   console.log(a); // undefined
   console.log(b); // b is not defined

   b = 1; // 在非严格模式下，给全局对象创建了一个新属性，但它不是变量
   var a = 2;
   ```

## 5.执行上下文栈

每当执行一段可执行代码时，即会进入一个执行上下文（EC）。执行上下文是一个抽象概念，用于与可执行代码概念进行区分。

每个执行上下文，都有三个重要属性：

- [变量对象（Variable object，VO）](https://github.com/AlfredYan/JavaScript-Note/blob/master/notes/JavaScript%E4%BD%9C%E7%94%A8%E5%9F%9F%E5%8F%8A%E9%97%AD%E5%8C%85.md#6变量对象) 
- [作用域链（Scope chain）](https://github.com/AlfredYan/JavaScript-Note/blob/master/notes/JavaScript%E4%BD%9C%E7%94%A8%E5%9F%9F%E5%8F%8A%E9%97%AD%E5%8C%85.md#7作用域链) 
- this

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

**函数上下文中的变量对象：** 在函数执行上下文中，VO是不能直接访问的，此时由活动对象（AO）扮演VO角色。**活动对象在进入函数上下文时被创建的** ，通过函数的arguments属性初始化。arguments属性的值是Arguments对象。

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

   ```javascript
   function test(a, b){
     var c = 10;
     function d(){};
     var e = function _e(){};
     (function x(){}); //函数表达式不会影响VO
   }

   test(10);

   //进入带有参数10的test函数上下文时，AO：
   AO(test) = {
     a: 10,
     b: undefined,
     c: undefined,
     d: <reference to FunctionDeclaration "d" >,
     e: undefined
   }
   ```

2. 代码执行

   前面那个例子在代码执行阶段，AO/VO会被修改为如下：

   ```javascript
   AO['c'] = 10;
   AO['e'] = <reference to FunctionExpression "_e">;
   ```


## 7.作用域链

每个上下文拥有自己的变量对象：对于全局上下文，它是全局对象自身；对于函数，则是活动对象。作用域链是内部上下文所有变量对象的列表。作用域链用来变量查询。

作用域链与一个执行上下文相关，变量对象的链用于在标识符解析中变量查找。

函数上下文的**作用域链在函数调用时创建** ，包含活动对象和这个函数内部的[[scope]]属性。

**Scope = AO|VO + [[scope]]** 

```javascript
activeExecutionContext = {
  VO: {...}, // or AO
  this: thisValue,
  Scope: [
    // Scope chain
    // 所有变量对象的列表
  ]     
};
  
Scope = [AO].concat([[scope]]);  
```

[[scope]]是所有父变量对象的层级链，处于当前函数上下文之上，**[[scope]]在函数被创建时被存储** ，是静态的，直到函数销毁。

**函数的生命周期：** 

1. **函数创建：** 

   ```javascript
   var x = 10;

   function foo() {
     var y = 20;
     console.log(x + y);
   }

   fooContext.AO = {
     y: undefined // undefined （在进入上下文时是20）
   }
   ```

2. **函数激活：** 

   标识符解析是一个处理过程，用来确定一个变量（或函数声明）属于哪个变量对象。

   标识符解析过程包含与变量名对应名对应属性的查找，即作用域中变量对象的连续查找，从最深的上下文开始，直到作用域链的最上层。

   ```javascript
   var x = 10;

   function foo() {
     var y = 20;
     
     function bar(){
       var z = 30;
       console.log(x + y + z);
     }
     
     bar();
   }

   foo(); // 60
   ```

   我们有如下的变量/活动对象，函数的[[scope]]属性即上下文的作用域链：

   全局上下文变量对象：

   ```javascript
   globalContext.VO === Global = {
     x: 10,
     foo: <reference to function>
   };
   ```

   在foo创建时，foo的[[scope]]属性：

   ```javascript
   foo.[[Scope]] = [
     globalContext.VO
   ];
   ```

   在foo被调用（进入上下文），foo上下文的活动对象是：

   ```javascript
   fooContext.AO = {
     y: 20,
     bar: <reference to function>
   };
   ```

   foo上下文的作用域链为：

   ```javascript
   // fooContext.Scope = fooContext.AO + foo.[[Scope]]

   fooContext.Scope = [
     fooContext.AO,
     globalContext.VO
   ];
   ```

   内部函数bar创建时，其[[Scope]]为：

   ```javascript
   bar.[[Scope]] = [
     fooContext.AO,
     globalContext.VO
   ];
   ```

   bar被调用时，bar上下文的活动对象为：

   ```javascript
   barContext.AO = {
     z: 30
   };
   ```

   bar上下文作用域链为：

   ```javascript
   // barContext.Scope = barContext.AO + bar.[[Scope]]

   barContext.Scope = [
     barContext.AO,
     fooContext.AO,
     globalContext.VO
   ];
   ```

   对x、y、z的标识符解析为：

   ```javascript
   // x
   barContext.AO // not found
   fooContext.AO // not found
   golbalContext.VO // found, 10

   // y
   barContext.AO // not found
   fooContext.AO // found, 20

   // z
   barContext.AO // found, 30
   ```

**通过函数构造函数创建的函数的[[Scope]]：** 通过函数构造创建的函数的[[scope]]属性总是唯一的全局对象。

```javascript
var a = 10;

function foo() {
  var y = 20;
  
  // 函数声明
  function barFD() {
    console.log(x);
    console.log(y);
  }
  
  // 函数表达式
  var barFE = function() {
    console.log(x);
    console.log(y);
  };
  
  // 使用函数构造函数创建函数
  var barFn = Function('console.log(x); console.log(y);');

  barFD(); // 10, 20
  barFE(); // 10, 20
  barFn(); // 10, y is not defined
  
}

foo();
```

**二维作用域链查找：** 当代码需要查找一个属性或者标识符时，首先会通过作用域链来查找相关的对象；如果对象未被找到，扣回根据对象的原型链来查找属性。（活动对象没有原型）

```javascript
// 例子1
function foo() {
  console.log(x);
}

Object.prototype.x = 10;

foo(); // 10
```

```javascript
// 例子2
// 活动对象没有原型
// 只有到达全局对象，它从Object.prototype继承而来
function foo() [
  var x = 20;
  
  function bar() {
    console.log(x);
  }
  
  bar();
]

Object.prototype.x = 10;

foo(); // 20
```

## 8.闭包

**自由变量：** 在函数中使用，但既不是函数参数也不是函数的局部变量的变量。

```javascript
function testFn() {
  var localVar = 10;
  
  function innerFn(innerParam){
    console.log(innerParam + lovalVar); // localVar为自由变量
  }

  return innerFn;
}

var someFn = testFn();
someFn(20); // 30 
```

在面向堆栈的编程语言中，函数的局部变量都是保存在栈上的，每当函数被调用时，这些变量和函数参数都会压入到栈上。当函数返回时，这些参数会从栈中移出。

对于采用面向栈模型来存储局部变量的系统而言，上面的那个例子中，当在外部对innerFn进行函数调用时，会发生错误，因为localVar已经不存在了。

上述问题取决于是否将函数以返回值返回，以及是或否将函数当函数参数使用。为了解决上述问题，就需要引入闭包。

**闭包：** 闭包是代码块和创建该代码块的上下文中数据的结合。

创建该函数的父级上下文的数据是保存在函数的内部属性[[scope]]中的。所有函数都是闭包，因为它们都是在创建的时候就保存了上次上下文的作用域链。不管函数后续是否被调用，[[scope]]在函数创建时就有了。

```javascript
// 例子1
var x = 10;

function foo() {
  console.log(x);
}

(function (funArg) {
  var x = 20;
  
  // 变量x在上下文中静态保存，在该函数创建时就保存了
  funArg(); // 10
})(foo);
```

```javascript
// 例子2
var x = 10;

function foo() {
  console.log(x);
}

// foo是闭包
foo: <FunctionObject> = {
  [[Call]]: <code block of foo>,
  [[Scope]]: [
    global: {
      x: 10
    }
  ],
  ... // 其他属性
};
```

**所有对象都引用一个[[Scope]]：** 同一个父上下文中创建的闭包是共有一个[[Scope]]属性的。当某个闭包对其中[[Scope]]的变量修改会影响到其他闭包对其变量的读取。

```javascript
// 例子1
var firstClosure;
var secondClosure;

function foo() {
  var x = 1;
  
  firstClosure = function() { return ++x };
  secondClosure = function() { return --x };
  
  x = 2 // 影响AO['x']，在2个闭包共有[[Scope]]中
  
  console.log(firstClosure()); // 3，通过第一个闭包的[[Scope]]
}

foo();

console.log(firstClosure()); // 4，通过第一个闭包的[[Scope]]
console.log(secondClosure()); // 3，通过第二个闭包的[[Scope]]
```

```javascript
// 例子2
var data = [];

for(var i = 0; i < 3; i++) {
  data[i] = function() {
    console.log(i);
  };
}

data[0](); // 3
data[1](); // 3
data[2](); // 3

activeContext.Scope = {
  ... // 其他变量对象
  {data: [...], i: 3} //活动对象
};
          
// 使用闭包修改循环，使其输出0、1、2
for(var i = 0; i < 3; i++) {
  data[i] = (function(x) {
    return function() {
      console.log(x);
    };
  })(i);
}

data[0](); // 0
data[1](); // 1
data[2](); // 2
```

**理论版本：** 

ECMAScript中，闭包指的是：

1. 从理论角度：所有函数都是闭包，因为它们都在创建的时候将上层上下文的数据保存起来了。
2. 从实践角度：
   - 即使创建它的上下文已经销毁，它仍然存在（比如，内部函数从父函数中返回）
   - 在代码中引用了自由变量

**闭包实例：** 

1. 数组排序，接受一个排序条件函数作为参数：

   ```javascript
   [1, 2, 3].sort(function(a, b) {
     ... // 排序条件
   });
   ```

2. 延迟调用：

   ```
   var a = 10;
   setTimeOut(function() {
     console.log(a);
   }, 1000);
   ```

3. 所有回调函数

4. 模块：必须有外部的封闭函数，该函数必须至少被调用一次。封闭函数必须返回至少一个内部函数。

   ```javascript
   var foo = (function myModule() {
     var something = "cool";
     var another = [1, 2, 3];
     
     function doSomething() {
       console.log(something);
     }
     
     function doAnother() {
       console.log(another.join(","));
     }
     
     return {
       doSomething: doSomething,
       doAnother: doAnother
     };
   })();

   foo.doSomething() // cool
   foo.doAnother() // 1,2,3
   ```


## 总结

1. 当查找的目的是对变量进行赋值时，会使用LHS查询；当查找的目的是获取变量的值，会使用RHS查询。
2. JavaScript采用词法作用域，作用域在定义时就确定了，即作用域由代码所写位置决定。
3. 函数声明与变量定义会被提升至代码顶部，函数声明比变量定义有更高的优先级。
4. 每当执行一段可执行代码时，就会进入一个执行上下文。执行上下文中包含：变量对象、this和作用域链。
5. 变量对象中包含：变量、函数声明和函数的形参。全局上下文的变量对象在进入执行上下文前就已经创建，在程序结束时终止。函数上下文的变量对象由活动对象扮演，在进入函数上下文时被创建。
6. [[Scope]]是所有父变量对象的层级链，在函数被创建时就被存储，是静态不变的。
7. 作用域链（Scope）在函数调用时创建，Scope = AO|VO + [[Scope]]。
8. 闭包是代码块和创建代码块上下文中数据的结合，闭包使得在函数被当做函数参数和以函数为返回值时，仍然能获取到函数中的自由变量。