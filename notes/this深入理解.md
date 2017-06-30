# this深入理解

## 1.this基本概念

**this基本概念：** this既不指向函数自身也不指向函数的词法作用域。this是在函数被调用时发生绑定的，所以this取决于**函数的调用方式（调用位置）** ，this指的是调用函数的那个对象。**this值在进入上下文时确定** ，并且在上下文运行期间永久不变。

**调用栈** ：为了到达当前执行位置所调用的所有函数，可以理解为一个函数调用链。调用位置就在当前正在执行的函数的前一个。

## 2.this绑定规则

1. **默认绑定：** 独立函数调用，可以看做是无法应用其他绑定规则时的默认规则。在严格模式下，无法使用默认绑定，this会绑定到undefined。在非严格模式下会被绑定到全局对象。

   ```javascript
   function foo() {
     return this;
   }
   console.log(foo() === window); // true

   function bar() {
     "use strict"; // 使用严格模式
     return this;
   }
   console.log(bar() === undefined) // ture
   ```

2. **隐式绑定：** 当函数引用有上下文对象时，隐式绑定会把函数调用中的this绑定到这个上下文对象。对象属性引用链中只有最后一层会影响调用位置。

   ```javascript
   function foo() {
     console.log(this.a);
   }

   var obj2 = {
     a: "obj2",
     foo: foo
   }

   var obj1 = {
     a: "obj1",
     obj2: obj2
   }

   obj1.obj2.foo(); // obj2，隐式绑定，此时this指向obj2
   ```

   被隐式绑定的函数会丢失绑定对象，使其会应用默认绑定（使用函数别名、回调函数等情况）

   ```javascript
   function foo() {
     console.log(this.a);
   }

   var obj = {
     a: "obj",
     foo: foo
   };

   var a = "window"
   var bar = obj.foo; // 函数别名

   // bar实际引用的是foo函数本身，因此此时是一个独立函数调用
   bar(); // window
   ```

3. **显示绑定：** 使用**call(thisObj, arg1, arg2, ...)** 或者**apply(thisObj, [argArray])** 方法，它们的第一个参数是一个对象，会将这个对象绑定到this，接着在调用函数时制定这个this。

   ```javascript
   function foo() {
     console.log(this.a);
   }

   var obj = {
     a: "obj"
   };

   foo.call(obj); // obj
   ```

   使用硬绑定**bind(thisObj, arg1, arg2, ...)** 可以解决丢失绑定的问题。bind方法会返回一个新函数，它把参数设置为this的上下文并调用原始函数。

   ```javascript
   function foo(something) {
     console.log(this.a, something);
   }

   var obj = {
     a: 2
   };

   var bar = foo.bind(obj);

   bar(3); // 2 3
   ```

4. **new绑定：** 使用[new操作符](https://github.com/AlfredYan/JavaScript-Note/blob/master/notes/JavaScript%E5%8E%9F%E5%9E%8B%E4%B8%8E%E5%8E%9F%E5%9E%8B%E9%93%BE.md#new的具体操作)调用函数，也可称为对函数的“构造调用”。使用new来调用函数时，会构造一个新的对象，并把这个新对象绑定到韩式调用的this。

   ```javascript
   function foo(a) {
     this.a = a;
   }

   var bar = new foo(2);
   console.log(bar.a); // 2
   ```

**绑定方式优先级：** new绑定 > 显示绑定 > 隐式绑定 > 默认绑定

在使用apply()来展开一个数组或者使用bind对参数进行柯里化（预先设置一些参数）时，会将null或者undefined作为this的绑定对象传入call、apply或bind时，这些值在调用时会被忽略，实际应用的是默认绑定。在这类情况下使用Object.create(null)能创建一个完全的空对象，比null或undefined更合适

```javascript
function foo(a, b) {
  console.log("a:" + a + ", b:" + b);
}

// 使用空集符号使代码可读性更高
var ∅ = Object.create(null);

// 数组展开成参数
foo.apply(∅, [2.3]); // a:2, b:3

//使用bind柯里化
var bar = foo.bind(∅, 2);
bar(3); // a:2, b:3
```

