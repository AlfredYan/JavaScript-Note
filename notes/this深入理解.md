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
   第三方库的许多函数，以及JavaScript和宿主对象环境中的许多内置函数，都提供了一个可选参数，被称为“上下文”（context），其作用和bind(...)一样，确保回调函数使用你指定的this。

   ```javascript
   function foo(el) {
     console.log(el, this.id);
   }

   var context = {
     id: "awesome"
   };

   // 调用foo(..)时，把this绑定到context
   [1, 2, 3].forEach(foo, context); // 1 awesome 2 awesome 3 awesome
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

**被忽略的this:** 在使用apply()来展开一个数组或者使用bind对参数进行柯里化（预先设置一些参数）时，会将null或者undefined作为this的绑定对象传入call、apply或bind时，这些值在调用时会被忽略，实际应用的是默认绑定。在这类情况下使用**Object.create(null)能创建一个完全的空对象** ，比null或undefined更合适

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

## 3.this词法（箭头函数）

ES6中有一种无法使用this绑定规则的特殊函数类型：箭头函数。箭头函数不使用"function"关键字定义，而是使用**“=>”** 定义的。箭头函数根据外层（函数或全局）作用域来决定this。

```javascript
function foo() {
  return (a) => {
    // this继承自foo()
    console.log(this.a)
  }
}

var obj1 = {
  a: 1
};

var obj2 = {
  a: 2
};

var bar = foo.call(obj1);
bar.call(obj2); // 1
```

foo内部创建的箭头函数会捕获调用时foo()的this。上例中，由于foo()的this被绑定到obj1，bar的this也被绑定到了obj1，**箭头函数的绑定无法修改** 。

## 4.从ECMAScript规范了解this

1. **引用类型（Reference）：** 

   Reference类型是用来解释delete、typeof及赋值的那个操作行为的。是“只存在于规范里的抽象类型”，为了更好的描述语言底层的行为逻辑而存在的。

   引用类型的值只有两种情况：

   1. 当处理一个标示符（变量名，函数名，函数参数名和全局对象中未识别的属性名）时
   2. 一个属性访问器（（.）语法或（[]）语法）

   ```javascript
   // 标示符
   var a = 10;
   function b() {}

   // 属性访问器
   foo.bar();
   foo['bar']();
   ```

   Reference由3个部分组成：

   - base value：属性所在对象或者就是EnvironmentRecord，它的值可能是undefined、Object、Boolean、String、Number 或者Environment Record中的一种。

   - referenced name：属性的名称。

   - strict reference：标志符，Boolean型。

     ```javascript
     var foo = 1;

     // foo的Reference：
     var fooReference = {
       base: EnvironmentRecord,
       name: 'foo',
       strict: false
     };

     var bar = {
       baz: function() {
         return this;
       }
     };

     bar.baz(); // bar

     // bza的Reference：
     var BazReference = {
       base: bar,
       name: baz,
       strict: false
     }
     ```

2. **获取Reference组成部分的方法：** 

   - GetBase：返回Reference的base value

   - IsPropertyReference：当base value是一个对象时，返回true；否则返回false。

   - GetValue：返回对象属性真正（具体）的值，**而不再是一个Reference** 。

     ```javascript
     // GetValue方法伪代码
     function GetValue(value) {
       
       if (Type(value) != Reference) {
         return value;
       }
       
       var base = GetBase(value);
       
       if (base === null) {
         throw new ReferenceError;
       }
       
       return base.[[Get]](GetPropertyName(Value));
     }
     ```

3. **确定this的值：** 

   在一个函数上下文中，this由调用者提供，由调用函数的方式来决定。如果**调用括号()的左边** 是引用类型的值，**this将设为引用类型值的base对象（base object）** ，在其他情况下，这个值为null，其值会被隐式转换为全局对象。

   **调用货号左边为引用类型：** 

   ```javascript
   // 标示符例子
   function foo() {
     return this;
   }

   foo(); // global

   // 调用括号左边为一个引用类型值，foo是一个标示符
   // 所以this设置为引用类型的base对象，即全局对象
   var fooReference = {
     base: global,
     name: 'foo'
   };
   ```

   ```javascript
   // 属性访问器例子
   var foo = {
     bar: funtion() {
       return this;
     }
   };

   foo.bar(); // foo

   var fooReference = {
     base: foo,
     name: 'bar'
   };
   ```

   由于应用类型不同的中间值，用表达式的不同形式激活同一个函数会有不同的this值。

   ```javascript
   // 在上面那个例子中，使用另一种表达式激活函数
   var baz = foo.bar;
   baz(); // global

   var bazReference = {
     base: global,
     name: 'baz'
   }
   ```

   **调用括号左边为非引用类型：**

   ```javascript
   // 例子中的函数对象不是引用类型对象（既不是标示符也不是属性访问器）
   // this值最终设为全局对象
   (function() {
     cosnole.log(this); // null => global
   })();
   ```

   ```javascript
   var foo = {
     bar: function() {
       console.log(this);
     }
   };

   // 在"()"运算符的返回中，未使用GetValue方法，所以返回的仍是引用类型
   // 所以this设为base对象，即foo
   (foo.bar) (); // foo

   // 赋值运算符（=）调用了GetValue方法，返回的函数对象
   // 所以this设为null， 隐式转换为global
   (foo.bar = foo.bar) ();// nul l => global

   // “||”运算符调用了GetValue方法，返回的值不再是引用类型
   // 所以this设为null， 隐式转换为global
   (false || foo.bar) (); // null => global

   // “,”运算符调用了GetValue方法，返回的值不再是引用类型
   // 所以this设为null， 隐式转换为global
   (foo.bar, foo.bar) (); // null => global
   ```

   **引用类型和this为null：** 

   当引用类型值的base对象是[活动对象](https://github.com/AlfredYan/JavaScript-Note/blob/master/notes/JavaScript%E4%BD%9C%E7%94%A8%E5%9F%9F%E5%8F%8A%E9%97%AD%E5%8C%85.md#6变量对象) 时，this会被设为null，但最终结果会隐式转换为global。比如，内部函数在父函数中被调用。

   ```javascript
   function foo() {
     function bar() {
       cosnole.log(this);
     }
     
     // 活动对象总是作为this返回，值为null，即AO.bar()相当于null.bar()
     bar(); // null => global (相当于AO.bar())
   }
   ```

## 5.总结

1. this是由激活上下文代码的调用者来提供，即调用函数的父上下文。this取决于调用函数的方式。
2. this绑定的方式有：默认绑定、隐式绑定、显示绑定、new绑定；优先级以此升高。
3. ES6中，箭头函数根据外层（函数或全局）作用域来决定this。
4. 如果调用“()”左边是引用类型，this设为引用类型值的base对象；如果不是引用类型，this设为null，这种情况下null会隐式转换成全局对象。

