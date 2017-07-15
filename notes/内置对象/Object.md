# Object

简介：学习Object和Object.prototype上的方法。（[参考](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object))

查看一个对象属性的方法：

```javascript
Object.getOwnPropertyNames(Object).sort().forEach(function(val) { console.log(val); });

"arguments" ×
"assign" √
"caller" ×
"create" √
"defineProperties" √
"defineProperty" √
"entries" √
"freeze" √
"getOwnPropertyDescriptor" √
"getOwnPropertyDescriptors" √
"getOwnPropertyNames" √
"getOwnPropertySymbols" √
"getPrototypeOf" √
"is" √
"isExtensible" √
"isFrozen" √
"isSealed" √
"keys" √
"length" ×
"name" ×
"preventExtensions" √
"prototype" √
"seal" √
"setPrototypeOf" √
"values" √
```

## 1.new Object()

使用**Object()构造函数** 时可以传入一个参数，如果参数是一个对象则，则直接返回这个对象；如果是一个原始类型的值，则返回该值对应的包装对象。

```javascript
var o1 = {a: 1};
var o2 = new Object(o1);
console.log(o1 === o2); // true

new Object(1) instanceof Number // true
```

在Object对象上添加方法：

1. 部署在Object对象本身（静态方法）
2. 部署在Object.prototype对象上（实例对象方法）

## 2.Object()

**Object()** 作为工具方法使用，可以将任意值转为对象，用于保证某个值一定为对象。

```javascript
// 返回一个空对象
Object() instanceof Object // true
Object(null) instanceof Object // true
Object(undefined) instanceof Object // true

Object(1) instanceof Number // true 相当于 new Number(1)
Object('a') instanceof String // true 相当于 new Stirng("a")
Object(true) instanceof Boolean // true 相当于 new Boolean(true)
```

如果传入的值本身就是一个对象，则直接返回该对象。该特性可以用于检测**函数传入的参数是否为一个对象** 。

```javascript
function isObject(val) {
  return val === Object(val);
}

isObject([1, 2]); // true
isObject(1); // false
```

## 3.Object.defineProperty() && Object.defineProperties()

**Obejct.defineProperty(obj, prop, descriptor)：** 直接在对象上新增或修改一个属性，并返回这个对象

- obj：将要被定义属性的对象
- prop：要定义或修改的属性名称
- descriptor：要定义或修改的[属性描述符](https://github.com/AlfredYan/JavaScript-Note/blob/master/notes/%E5%AF%B9%E8%B1%A1.md#4属性数据描述符) 。当描述符中省略某些字段时：writable、enumerable和configurable默认为false；get、set和value默认为undefined。（**数据描述符不能与访问描述符混合使用** ）

```javascript
var o = { };

Object.defineProperty(o, "a", {
  value : 1,
  enumerable : true,
  configurable : true,
});
o.a = 2; // 非严格模式下静默失败
console.log(o.a); // 1

var bValue;
Object.defineProperty(o, "b", {
  get: function() {
    return bValue;
  },
  set: function(newValue) {
    bValue = newValue;
  }
});
o.b = 1
console.log(o.b); // 1

// 数据描述符和存取描述符不能混合使用(value和writable不能与get、set共存)
Object.defineProperty(o, "conflict", {
  value: 1, 
  get: function() { 
    return value; 
  } 
}); // TypeError: Cannot both specify accessors and a value or writable attribute
```

**Object.defineProperties(objs, props)：** 在一个对象上新增或修改属性，并返回这个对象。

- obj：将被定义属性的对象
- prop：该对象的键值对定义了将要成为对象的属性的具体配置。（{prop1: descriptor1, prop2: discript2, ...}）

```javascript
var o = { };
var bValue;
Object.defineProperties(o, {
  a: {
    value: 1,
    writable: true
  },
 b: {
   get: function() {return bValue;},
   set: function(newValue) {bValue = newValue}
 }
});
```

## 4.Object.getOwnPropertyDescriptor()

**Object.getOwnPropertyDescriptor(obj, prop)：** 返回对象某个自身属性的属性描述符。

- obj：将查看属性所在对象
- prop：将查看的属性名

```javascript
var o = { };
Object.defineProperty(o, "a", {
  value : 1,
  writable: true,
  configurable : true,
})
Object.getOwnPropertyDescriptor(o, "a"); 
// Object {value: 1, writable: true, enumerable: false, configurable: true}
```

**Object.getOwnPropertyDescriptors(obj)：** 返回对象所有自身属性的属性描述符。

## 5.控制对象状态的方法：extension、seal、freeze

**Object.preventExtensions(obj) ：** 让一个对象变得不可扩展，即无法增加新的自身属性。

- obj：将要变得无法扩展的对象

**Object.isExtensible(obj)：** 判断一个对象是否可扩展（是否可以新增自身属性）。

- obj：将要被检测的对象

```javascript
var o = { };

Object.preventExtensions(o);
o.a = 1; // 非严格模式下，静默失败
Object.isExtensible(o); // false
```

**Object.seal(obj)：** 让一个对象密封，并返回被密封后的对象。密封对象是指：对象无法新增属性、无法删除属性、无法修改属性的属性描述符，但可以修改writable为true属性的值。（即preventExtensions() + 将所有属性的configurable设置为false）

- obj：将要被密封的属性

**Object.isSealed(obj)：** 判断对象是否被密封。

- obj：将要被检测的对象

```javascript
var o = { a: 1 };

Object.seal(o);
o.a = 2;
o.b = 2;// 静默失败
delete o.a;// 静默失败
console.log(o.a); // 2
console.log(o.b); // undefined
Object.isSealed(o); // true
```

**Object.freeze(obj)：** 让一个对象冻结，并返回被冻结对象。冻结对象是指：无法新增属性、无法删除属性、无法修改属性的属性描述符，也无法修改已有属性的值（即seal() + 将所有属性的writable设置为false）。

- obj：将要被冻结的属性

**Object.isFrozen(obj)：** 判断对象是否被冻结。

-  obj：将要被检测的对象

```javascript
var o = { a: 1 };

Object.freeze(o);
o.a = 2;
console.log(o.a); // 1
Object.isFrozen(o); // true
```

## 6.Object.getOwnPropertyNames() & Object.keys()

**Object.getOwnPropertyNames(obj)：** 返回由指定对象所有**自身属性（无论是否可枚举，但不包括原型链）** 的属性名所组成的**数组** 。

- obj：被指定的对象

**Object.keys(obj)：** 返回由指定对象的所有**可枚举自身对象（不包括原型链）** 的属性名所组成的**数组** 。

- obj：被指定的对象

```javascript
var o = {
  a: 1,
  b: 2
};
Object.defineProperty(o, "c", {
  value: 3,
  enumerable: false
});

Object.getOwnPropertyNames(o);// ["a", "b", "c"]
Object.keys(o);// ["a", "b"]
```

## 7.Object.getOwnPropertySymbols()

**Object.getOwnPropertySymbols(obj)：** 返回一个数组，该数组包含指定对象所有自身的symbol属性键。

- obj：指定的对象

```javascript
var o = { };
var a = Symbol("a");
var b = Symbol.for("b");

o[a] = "a";
o[b] = "b";
o.c = "c"

Object.getOwnPropertySymbols(o); // [Symbol(a), Symbol(b)]
Object.getOwnPropertyNames(o);// ["c"]
```

## 8.Object.entries() & Object.values()

**Object.entries(obj)：** 返回一个指定对象的自身的可枚举对象的[key, value]对的数组。

- obj：指定对象

**Object.values(obj)：** 返回由指定对象的所有可枚举自身属性的值所组成的数组。

- obj：指定对象

```javascript
var o = {
  a: 1,
  b: 2
};
Object.defineProperty(o, "c", {
  value: 3,
  enumerable: false
});

Object.entries(o); // [["a", 1], ["b", 2]]
Object.values(o); // [1, 2]
```

## 9.Object.is()

**Object.is(value1, value2)：** 判断两个值是否为相同的值。它与严格比较运算符（===）的行为基本一致，与``===`` 不同的是，``is`` 会认为+0与-0不相等，以及NaN等于自身。

- value1：需要比较的第一个值
- value2：需要比较的第二个值

```javascript
var obj = { };

Object.is({}, {}); // false
Object.is(obj, obj); // true

console.log(+0 === -0); // true
Object.is(+0, -0); // false
Object.is(+0, +0); // true

console.log(NaN === NaN); // false
Object.is(NaN, NaN); // true
```

## 10.Obejct.assign()

**Object.assign(target, …sources)：** 将所有可枚举的自身属性的值从一个或多个源对象（会跳过值为null或undefined的源对象），复制到目标对象，并返回目标对象。属于浅复制，并且不会复制属性定义（writable、configurable...）。

- target：目标对象
- sources：源对象

```javascript
var o1 = {a: 1};
var o2 = {b: 2};

Object.assign({}, o1, o2); // {a: 1, b: 2}
```

## 11.Object.create()

**Object.create(proto, [propertiesObject])：** 使用的原型对象和其属性创建一个新对象。

- proto：一个对象，作为新创建对象的原型。
- propertiesObject：可选。该对象是一组属性与值。该对象的属性名将作为新对象的属性名，值为属性描述符。

```javascript
var o;

o = Object.create(Object.prototype, {
  // foo会成为所创建对象的数据属性
  foo: { writable:true, configurable:true, value: "hello" },
  // bar会成为所创建对象的访问器属性
  bar: {
    configurable: false,
    get: function() { return 10 },
    set: function(value) { console.log(value) }
}})
```

## 12.Object.getPrototypeOf() & Object.setPrototypeOf()

**Object.getPrototypeOf(obj)：** 返回指定对象的原型（即指定对象内的[[prototype]]值）。

- obj：指定对象

**Object.setPrototypeOf(obj, prototype)：** 设置指定对象的原型（即[[prototype]]属性）到另一个对象或null。

- obj：指定对象
- prototype：指定对象的新原型，如果不是对象或null，则什么都不做。

```javascript
var o;
function test(){ }
test.prototype.example = "example";

o = Object.create(Object.prototype);

Object.getPrototypeOf(o); // Object {...}
Object.setPrototypeOf(o, test.prototype);
Object.getPrototypeOf(o);// test {...}
```

## 13.Object.prototype对象

Object.prototype对象中主要包含以下方法（所有Object的实例对象都继承了这些方法）：

1. **obj.valueOf()：** 返回指定对象的原始值。如果该对象没有原始值，则返回对象自身。每个内置对象都会覆盖这个方法，来返回一个合适的值，当然也可以为自定义对象一个创建valueOf()来覆盖它。**在JS自动类型转换时会默认调用这个方法** 。

   ```javascript
   var o;

   // 对象有原始值时，返回原始值
   o = new Number(1);
   o.valueOf() // 1

   // 对象无原始值时，返回对象本身
   o = new Object();
   o.valueOf() === o // true

   // 自动类型转换时的默认调用
   console.log(10 - o); // NaN

   // 自定义valueOf()覆盖后，自动类型转换时的默认调用
   o.valueOf = function() { return 1}
   console.log(10 - o); // 9
   ```

2. **obj.toString()：** 返回一个表示该对象的字符串。该字符串说明对象的类型。**在JS自动类型转换时会默认调用这个方法** 。

   ```javascript
   var o;

   o = new Object();
   o.toString(); // "[object Object]"

   o.toString = function() { return "Hello"; }
   console.log(o + " World"); // Hello World
   ```

   使用toString()判断数据类型：因为实例对象可能会自定义toString()方法来覆盖Object.prototype.toString()，所以使用Object.prototype.toString.call(obj)。（[参考](http://javascript.ruanyifeng.com/stdlib/object.html#toc5)） 

   ```javascript
   var toString = Object.prototype.toString;

   toString.call(1); // "[object Number]"
   toString.call("a"); // "[object String]"
   toString.call(true); // "[object Boolean]"
   toString.call(undefined); // "[object Undefined]"
   toString.call(null); // "[object Null]"
   toString.call([]); // "[object Array]"
   toString.call(function() {}); // "[object Function]"
   toString.call(new Error()); // "[object Error]"
   toString.call(new Date()); // "[object Date]"
   toString.call(/a/); // "[object RegExp]"
   toString.call({ }); // "[object Obejct]"

   function testType() {
     return toString.call(arguments);
   }
   testType() // "[object Arguments]"
   ```

3. **obj.hasOwnProperty(prop)：** 判断某对象是否具有指定属性作为自身属性，返回一个布尔值。

   - prop：将被判断的属性（指定属性）

   ```javascript
   var o;
   o = new Object();
   o.prop = "exists";
   o.deleteProp = function() {
     delete o.prop;
   }

   o.hasOwnProperty("prop"); // true
   o.deleteProp();
   o.hasOwnProperty("prop"); // false
   ```

4. **prototypeObj.isPrototypeOf(obj)：** 判断一个对象(prototypeObj)是否存在于另一个对象(obj)的原型链上，返回一个布尔值。

   - obj：在该对象原型上寻找

   ```javascript
   var o;
   o = new Object();
   Object.prototype.isPrototypeOf(o); // true

   function test() {}
   Object.setPrototypeOf(o, test.prototype)
   test.prototype.isPrototypeOf(o); // true
   Object.prototype.isPrototypeOf(test); // true
   ```

5. **obj.prototypeIsEnumerable(prop)：** 判断某属性是否为当前对象的可枚举自身属性，返回一个布尔值。

   - prop：将被判断的属性名

   ```javascript
   var o;
   o = ["is enumerable"];

   o.propertyIsEnumerable(0); // true
   o.propertyIsEnumerable("length"); // false
   ```

