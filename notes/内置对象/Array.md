# Array

简介：学习Array对象的属性、方法和实例方法。（[参考](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array) ）

Array对象中的属性与方法：

```javascript
Object.getOwnPropertyNames(Array).sort().forEach(function(val) { console.log(val); });

"arguments" ×
"caller" ×
"from" √
"isArray" √
"length" ×
"name" ×
"of" √
"prototype" √
```

## 1.简介

Array对象是JavaScript中的内置对象，同时也是一个构造函数，可以用来生成新的数组(无论是否使用``new`` 关键字)。

```javascript
// 生成一个3个元素的数组，每个位置都是空值。
var arr = new Array(3);
arr; // [undefined × 3]

// 等价于
arr = Array(3);
arr; // [undefined × 3]
```

Array()的参数不同会有导致它的行为不一致：

1. 无参数时，返回一个空数组
2. 单个参数时：
   - 单个**合法正整数** 参数，返回``length属性`` 为该正整数的数组，每个位置都为空值。
   - 单个**非法数值** 参数，报``RangeError`` 错误。
   - 单个**非数值** 参数，返回该参数作为元素的新数组。
3. 多个参数时，返回这些参数作为元素的新数组。

## 2.Array.form()

**Array.form(arrayLike[, mapFn[, thisArg]])：** 从一个类数组对象或可迭代的对象中创建一个新的数组实例。

- arrayLike：将要转换为数组实例的类数组对象或可迭代对象。
- mapFn：可选。生成的数组会经过该函数的处理再返回。
- thisArg：可选。执行mapFn时，this的指向。

```javascript
var arg = (function() {return arguments})("a", "b", "c")
var obj = {
    a: "A",
    b: "B",
    c: "C"
};

var arr = Array.from(arg, function(val, key) {
    return key + val + this[val];
}, obj)

arr; // ["0aA", "1bB", "2cC"]
```

## 3.Array.isArray()

**Array.isArray(obj)：** 判断传递的值是否为Array，返回true或false。

- obj：将要被检测的值。

```javascript
var arr = ["a", "b"];
var obj = {
    a: "a",
  	b: "b"
};

Array.isArray(arr); // true
Array.isArray(obj); // false
```

## 4.Array.of()

**Array.of(element0[, element1[, elementN]])：** 创建一个具有可变数量参数的新数组实例。

- elementN：任意个参数，将按顺序称为返回的数组中的元素。

Array.of()方法与Array()构造函数的区别在于处理整数参数。当参数为单个正整数，Array.of()方法会返回一个以该正整数为元素的数组；Array()构造函数则会返回一个``length属性`` 为该正整数的数组，每个位置都为空值。

```javascript
var arr1 = Array.of(1.5);
arr1; // [1.5]

var arr2 = Array.of(3);
var arr3 = new Array(3);
arr2; // [3]
arr3; // [undefined × 3]
```

## 5.Array.prototype对象

Array的实例方法：

```javascript
Object.getOwnPropertyNames(Array.prototype).sort().forEach(function(val) { console.log(val); });

"concat" ×
"constructor" ×
"copyWithin" √
"entries" √
"every" √
"fill" √
"filter" √
"find" √
"findIndex" √
"forEach" √
"includes" √
"indexOf" √
"join" √
"keys" √
"lastIndexOf" √
"length" ×
"map" √
"pop" √
"push" √
"reduce" √
"reduceRight" √
"reverse" √
"shift" √
"slice" ×
"some" √
"sort" √
"splice" √
"toLocaleString"
"toString" √
"unshift" √
```

## 6.arr.toString() & valueOf()方法

**Array.prototype.toString()：** 返回一个表示该数组及其元素的字符串。

**valueOf()** 方法，继承自Object.prototype，返回对象的原始值。对于Array来说，就是返回数组本身。

```javascript
function foo() {}
var obj = {a: "hello"}
var arr = ["a", 1, ["b", "c"], obj, foo];

arr.toString(); // "a,1,b,c,[object Object],function foo() {}"

arr.valueOf === Object.prototype.valueOf // true
arr.valueOf(); // ["a", 1, ["b", "c"], [object Object] {a: "hello"}, function foo() {}]
```

##7.arr.push() && arr.pop()

**Array.prototype.push(element1, …, elementN)：** 将一个或多个元素添加到数组的末尾，并返回数组的新长度。该方法会改变原数组。

- elementN：被添加到数组末尾的元素。

```javascript
var arr = ["a", "b", "c"];
function foo() {}

arr.push("d");
arr.push(foo);
arr; // ["a", "b", "c", "d", function foo() {}]
```

push()方法在与call()方法或apply()方法一起使用时，可应用在向某些类数组对象上。push方法根据``length属性`` 来决定从哪里插入给定值。如果``length属性`` 不能被转为一个数值，则插入的索引为0。如果``length属性`` 不存在，则会自动创建它。

push()方法与apply()方法结合使用，将第二个数组中所有元素添加到第一个数组：

```javascript
var arr1 = ["a", "b"];
var arr2 = [[1, 2], foo];
function foo() {}

// 合并两个数组
// 等价于 Array.prototype.push.apply(arr1, arr2)
arr1.push.apply(arr1, arr2);
arr1; // ["a", "b", [1, 2], function foo() {}]
```

push()方法与call()方法结合使用，向类数组对象末尾添加元素：

```javascript
// 存在可转为数值的length属性
var arrLike1 = {
    length: 1,
  	foo: "foo"
}
Array.prototype.push.call(arrLike1, "a");
arrLike1; // {1: "a", length: 2, foo: "foo"}

// 存在不可转为数值的length属性
var arrLike2 = {
    length: "notNum",
  	foo: "foo"
}
Array.prototype.push.call(arrLike2, "a");
arrLike2; // {0: "a", length: 1, foo: "foo"}

// 不存在length属性
var arrLike3 = {
  	foo: "foo"
}
Array.prototype.push.call(arrLike3, "a");
arrLike3; // {0: "a", foo: "foo", length: 1}

// push()方法无法用在某些类数组对象上，比如字符串。
var str = "abcd";
Array.prototype.push.call(str, "e"); // TypeError
```

**Array.prototype.pop()：** 删除数组的最后一个元素，并返回该元素的值。如果为空数组，则返回``undefined`` 。此方法会改变原数组。

```javascript
var arr = ["a", "b", "c"];

arr.pop(); // "c"
arr; // ["a", "b"]

// 对于空数组，pop()方法会返回undefined
[].pop(); // undefined
```

***注：*** *push()方法和pop()方法结合使用，可以构成“后进先出”的栈结构（stack）。* 

## 8.arr.unshift() & arr.shift()

**Array.prototype.unshift(element1, …, elementN)：** 将一个或多个元素添加到数组的开头，并返回数组的新长度。

- elementN：被添加到数组开头的元素。

```javascript
var arr = ["e", "f", "g"];

arr.unshift("d");
arr.unshift("a", "b", "c");
arr; // ["a", "b", "c", "d", "e", "f", "g"]
```

与push()方法类似，unshift()方法在与call()方法或apply()方法结合使用时，可应用在某些类数组对象上。

unshift()方法与apply()方法结合，合并2个数组：

```javascript
var arr1 = ["a", "b"];
var arr2 = ["c", "d"];

// 等价于 Array.prototype.unshift.apply(arr2, arr1)
arr2.unshift.apply(arr2, arr1);
arr2; // ["a", "b", "c", "d"]
```

unshift()方法与call()方法结合，向某些类数组对象开头插入元素：

```javascript
// 存在length属性的对象
var arrLike1 = {
	length: 1,
  	0: "b",
  	foo: "foo"
}
Array.prototype.unshift.call(arrLike1, "a");
arrLike1; // {0: "a", 1: "b", length: 2, foo: "foo"}

// 不存在length属性的对象
var arrLike2 = {
  	1: "d",
  	foo: "foo"
}
Array.prototype.unshift.call(arrLike2, "c");
arrLike2 // {0: "c", 1: "d", foo: "foo", length: 1}
Array.prototype.unshift.call(arrLike2, "a", "b");
arrLike2 // {0: "a", 1: "b", 2: "c", foo: "foo", length: 3}
```

**Array.prototype.shift()：** 删除数组的第一个元素，并返回该元素。如果数组为空则返回``undefined`` 。此方法会改变原数组。

```java
var arr = [1, "a", "b"];

arr.shift(); // 1
arr; // ["a", "b"]

// 对于空数组，shift()方法会返回undefined
[].shift(); // undefined
```

***注：*** *push()方法和shift()方法结合使用，可以构成“先进先出”的队列结构（queue）。* 

## 9.arr.splice()

**Array.prototype.splice(start[, deleteCount[, item1, item2, ...]])：** 对一个数组删除现有元素，添加新元素或替换现有元素。返回被删除的元素组成的数组，如果没有被删除的元素，则返回空数组。此方法会改变原数组。

- start：指定修改的开始位置。如果超过数组长度，则从数组末尾开始添加元素；如果是负值，则视为该负值与数组长度的和。
- deleteCount：可选。整数，表示要删除的元素个数。为0时，不删除元素。如果大于start之后的元素总和，则start之后的所有元素都将被删除（包括start位置）。被忽略时，相当于数组长度减去start。
- item1, item2, …：可选。从start位置开始，要添加到数组的新元素。

```javascript
// 将deletCount设为0，从指定位置插入新元素
var arr1 = ["a", "b", "c", "d", "e"];
arr1.splice(2, 0, "newItem1", "newItem2"); // []
arr1; // ["a", "b", "newItem1", "newItem2", "c", "d", "e"]
arr1.splice(-3, 0, "newItem3"); // []
arr1; // ["a", "b", "newItem1", "newItem2", "newItem3", "c", "d", "e"]

// 替换元素
var arr2 = ["a", "b", "c", "d", "e"];
arr2.splice(1, 2, "newC", "newD"); // ["b", "c"]
arr2; // ["a", "newC", "newD", "d", "e"]

// 删除元素
var arr3 = ["a", "b", "c", "d", "e"];
arr3.splice(2, 2); // ["c", "d"]
arr3; // ["a", "b", "e"]
// 当删除的数量大于start后元素的总和时，删除start后所有元素
arr3.splice(1, 10); // ["b", "e"]
arr3; // ["a"]
```

## 10.arr.concat() & arr.slice()

**Array.prototype.concat(valueN)：** 合并2个或多个数组，不改变原数组，并返回一个新数组。

- valueN：与原数组合并的数组或非数组值。在忽略该参数时，返回当前数组的一个**浅拷贝** 。

```javascript
var arr1 = ["Hello"];
var arr2 = ["world"];
var str = "str";
var num = 10;
var obj = {a: "a"};
var arr3 = [obj];

// 合并数组
arr1.concat(arr2); // ["Hello", "world"]

// 合并数组和非数组值
arr1.concat(arr2, num, obj); // ["Hello", "world", 10, {a: "a"}]

// 无参数时，返回原数组的浅拷贝
var newArr = arr3.concat();
newArr === arr3 // false
newArr[0] === arr3[0] // true

// 结合call()方法，将对象合并为数组
Array.prototype.concat.call(obj, {b: "b"}); // [{a: "a"}, {b: "b"}]
```

**Array.prototype.slice(begin, end)：** 将从begin到end（不包括end）d的部分浅拷贝到一个新的数组，并返回该新数组。原数组不会被改变。

- begin：可选。从该索引处开始浅拷贝原数组的元素，默认为0。为负值时，表示倒数的第几个元素。
- end：可选。在该索引处结束浅拷贝原数据组的元素。不包括自身。被省略或大于数组长度时，一直提取到原数组末尾。为负值时，表示倒数的第几个元素。

```javascript
var arr = ["a", "b", "c", "d", "e"];

arr.slice(); // ["a", "b", "c", "d", "e"]
arr.slice(0); // ["a", "b", "c", "d", "e"]
arr.slice(1, 3); // ["b", "c"]
arr.slice(1, 10); // ["b", "c", "d", "e"]
arr.slice(-3, -1); // ["c", "d"]
```

slice()方法与call()方法结合使用，可以将类数组对象转为真正的数组：

```javascript
var obj = {
    0: "a",
  	1: "b",
  	2: "c",
  	length: 3
};
var arg = (function() {return arguments}) ("a", "b", "c", "d");
var str = "abcdefg";

Array.prototype.slice.call(obj); // ["a", "b", "c"]
Array.prototype.slice.call(arg); // ["a", "b", "c", "d"]
Array.prototype.slice.call(str); // ["a", "b", "c", "d", "e", "f", "g"]
```

## 11.arr.copyWithin()

**Array.prototype.copyWithin(target, start, end)：** 浅拷贝数组的一部分到同一数组的另一位置，并返回改变后的数组。

- target：复制被浅拷的贝元素到该位置。默认为0，如果为负值，从末尾开始计算。如果大于数组的长度，将不会发生拷贝。如果在start之后，被浅拷贝的元素将被修改以符合数组的长度。
- start：浅拷贝元素的起始位置。默认为0，如果为负值，从末尾开始计算。
- end：浅拷贝元素的结束位置，但不包括该位置。默认为数组的长度，如果为负值，从末尾开始计算。。

```javascript
[1, 2, 3, 4, 5].copyWithin(2); // [1, 2, 1, 2, 3]
[1, 2, 3, 4, 5].copyWithin(-2); // [1, 2, 3, 1, 2]
[1, 2, 3, 4, 5].copyWithin(1, 2); // [1, 3, 4, 5, 5]
[1, 2, 3, 4, 5].copyWithin(1, 2, 4); // [1, 3, 4, 4, 5]
[1, 2, 3, 4, 5].copyWithin(-2, -3, -1); // [1, 2, 3, 3, 4]

var arrLike = {
    3: 1,
  	length: 5
}
Array.prototype.copyWithin.call(arrLike, 1, 3); // {1: 1, 3: 1, length: 5}
```

## 12.arr.fill()

**Array.prototype.fill(value, start, end)：** 用一个固定值填充一个数组中从start到end内的全部元素。返回修改后的数组。

- value：用来填充数组元素的值。
- start：可选。起始索引，默认值为0。如果为负值，则相当于数组长度与该负值之和。
- end：可选。终止索引，默认值为数组长度。如果为负值，则相当于数组长度与该负值之和。

```javascript
[1, 2, 3, 4, 5].fill(2); // [2, 2, 2, 2, 2]
[1, 2, 3, 4, 5].fill(2, 1, 3); // [1, 2, 2, 4, 5] 
[1, 2, 3, 4, 5].fill(2, -4, -2); // [1, 2, 2, 4, 5]
[1, 2, 3, 4, 5].fill(2, NaN, NaN); // [1, 2, 3, 4, 5]

Array(5).fill(2); // [2, 2, 2, 2, 2]

var arrLike = {
  	0: 1,
    length: 5
}
Array.prototype.fill.call(arrLike, 2, 1); // {0: 1, 1: 2, 2: 2, 3: 2, 4: 2, length: 5}
```

## 13.arr.reverse()

**Array.prototype.reverse()：**将数组中的元素位置颠倒，返回改变后的数组。该方法会改变原数组。

```javascript
var arr = ["d", "c", "b", "a", 4, 3, 2, 1];
arr.reverse(); // [1, 2, 3, 4, "a", "b", "c", "d"]
```

## 14.arr.join()

**Array.prototype.join(separator)：** 将数组或类数组对象的所有元素连接到一个字符串中，并返回这个字符串。如果元素是``undefined`` 或``null`` ，则转化为空字符串。

- separator：指定一个字符串来分割数组的每个元素。默认为”,“。如果是空字符串，则元素之间没有任何字符。

```javascript
function foo() {}
var obj = {b: "b"};
var arr = [1, "a", ["innerArr"], foo, undefined, null, obj];
arr.join(); // "1,a,innerArr,function foo() {},,,[object Object]"
arr.join("-"); // "1-a-innerArr-function foo() {}---[object Object]"
```

join()方法与call()方法结合，应用于某些类数组对象上：

```javascript
Array.prototype.join.call("Hello World", "-"); // "H-e-l-l-o- -W-o-r-l-d"

var obj = {
    0: "a",
  	2: "b",
  	length: 3
};
Array.prototype.join.call(obj, "-"); // "a--b"
```

## 15.arr.indexOf() & arr.lastIndexOf()

**Array.prototype.indexOf(searchElement[, fromIndex])：** 返回在数组中可以找到searchElement的第一个索引。如果不存在，返回-1。内部使用``===`` 进行比较。

- searchElement：要查找的元素。
- fromIndex：可选。开始查找的位置。默认值为0。如果大于或等于数组长度，则意味着不会在数组里查找。如果为负数，则从末尾开始计算。

```javascript
var arr = ["a", "b", "c", "d", "e"];

arr.indexOf("a"); // 0
arr.indexOf("c", 2); // 2
arr.indexOf("c", 3); // -1
arr.indexOf("c", -4); // 2
```

**Array.prototype.lastIndexOf(searchElement[, fromIndex])：** 返回在数组中找到searchElement的最后一个索引。从数组的后面向前面查找。如果不存在，返回-1。内部使用``===`` 进行比较。

- searchElement：要查找的元素。
- fromIndex：可选。从该位置开始逆向查找。默认为数组长度减1。如果大于或等于数组长度，则整个数组都会被查找。如果为负数，则视为从数组末尾向前偏移。

```javascript
var arr = ["a", "b", "c", "a", "b", "c"];

arr.lastIndexOf("a"); // 3
arr.lastIndexOf("a", 2); // 0
arr.lastIndexOf("c", 2); // 2
arr.lastIndexOf("c", -2); // 2
```

由于两个方法内部都是使用``===`` 进行比较的，而``NaN`` 是唯一一个不等于自身的值，所以这两个方法无法确定数组中是否包含``NaN`` 。

```javascript
var arr = [NaN];

arr.indexOf(NaN); // -1
arr.lastIndexOf(NaN); // -1
```

## 16.arr.includes

**Array.prototype.includes(searchElement[, fromIndex])：** 判断一个数组是否包含一个指定的值。如果是返回``true`` ，否则返回``false`` 。

- searchElement：需要查找的元素。
- fromIndex：可选。从该索引开始查找searchElement。默认为0。如果为负值，则相当于数组长度加上该负值。

```javascript
var arr = ["a", "b", "c", "d", "e"];

arr.includes("b"); // true
arr.includes("b", 2); // false
arr.includes("b", 10); // false
arr.includes("c", -3); // true
```

## 17.arr.every() & arr.some()

**Array.prototype.every(callback[, thisArg])：** 检测数组中的所有元素是否都通过callback的测试。当所有元素都通过时返回``true`` ，否则返回``false`` 。

- callback：用来测试每个元素是否通过的函数。该函数有三个参数：元素值、元素的索引、原数组。
- thisArg：执行callback时的this指向。

**Array.prototype.some(callback[, thisArg])：** 检测数组中是否有某些元素能通过callback的测试。当存在元素能通过时返回``true`` ，否则返回``false`` 。

- callback：用来测试每个元素是否通过的函数。该函数有三个参数：元素值、元素的索引、原数组。
- thisArg：执行callback时的this指向。

```javascript
var arr = [1, 2, 3, 4, 5, 6];
var a = 7;
var b = 0;
var obj = {
    a: 5,
  	b: 3
}
function compare(val, index, arr){
    return val < this.a && val > this.b;
}

arr.every(compare); // true
arr.every(compare, obj); // false

arr.some(compare); // true
arr.some(compare, obj); // true
```

## 18.arr.entries() & arr.keys()

**Array.prototype.entries()：** 返回一个新的Array Iterator对象，该对象包含数组中每个索引的键值对。

**Array.prototype.keys()：** 返回一个新的Array Iterator对象，该对象包含数组中的每个索引的键值。

```javascript
var arr = ["a", "b", "c"];

var entries = arr.entries();
entries; // Array Iterator {}
entries.next().value; // [0, "a"]
entries.next().value; // [1, "b"]
entries.next().value; // [2, "c"]
entries.next().value; // undefined
// 使用for...of循环
for(var entry of entries) {
    console.log(entry);
}
// [0, "a"]
// [1, "b"]
// [2, "c"]


var keys = arr.keys();
keys; // Array Iterator {}
keys.next().value; // 0
keys.next().value; // 1
keys.next().value; // 2
keys.next().value; // undefined
// 使用foo...of循环
for(var key of keys) {
    console.log(key);
}
// 0
// 1
// 2
```

## 19.arr.find() & arr.findIndex()

**Array.prototype.find(callback[, thisArg])：** 返回数组中满足callback的第一个元素的值，否则返回``undefined`` 。

- callback：在数组每一项上执行的函数。该函数有三个参数：元素值、元素的索引、原数组。
- thisArg：执行callback时的this指向。

**Array.prototype.findIndex(callback[, thisArg])：** 返回数组中满足callback的第一个元素的索引值，否则返回-1。

- callback：在数组每一项上执行的函数。该函数有三个参数：元素值、元素的索引、原数组。
- thisArg：执行callback时的this指向。

```javascript
var arr = [1, 2, 3, 4, 5, 6];
var a = 3;
var obj = {
    a: 5
}
function find(val, index, arr) {
    return val > this.a;
}

arr.find(find); // 4
arr.findIndex(find); // 3

arr.find(find, obj); // 6
arr.findIndex(find, obj); // 5
```

## 20.arr.reduce() & arr.reduceRight()

**Array.prototype.reduce(callback, initialValue)：** 对累加器和数组中的每个元素(**从左到右** )应用一个函数，将其减少为单个值。返回累计处理的结果。

- callback：执行数组中每一项的函数，包含4个参数：
  - accumulator：上一次调用callback时，返回的值。或者是提供的初始值(initialValue)。
  - currentValue：数组中正在处理的元素。
  - currentIndex：数组中正在处理的元素的索引值。如果有initialValue，从0开始，否则从1开始。
  - array：原数组。
- initialValue：可选。用于第一次调用callback时的第一个参数(accumulator)。如果没有设置初始值，则将数组中的第一个元素作为初始值。空数组调用reduce()方法时没有设置初始值将会报错。

**Array.prototype.reduceRight.reduce(callback, initialValue)：** 对累加器和数组的每个值(**从右到左** )应用一个值，将其减为单个值。返回累计处理的结果。

- callback：执行数组中每一项的函数，包含4个参数：
  - accumulator：上一次调用callback时，返回的值。或者是提供的初始值(initialValue)。
  - currentValue：数组中正在处理的元素。
  - currentIndex：数组中正在处理的元素的索引值。如果有initialValue，从0开始，否则从1开始。
  - array：原数组。
- initialValue：可选。用于第一次调用callback时的第一个参数(accumulator)。如果没有设置初始值，则将数组中的第一个元素作为初始值。空数组调用reduce()方法时没有设置初始值将会报错。

```javascript
var arr = [1, 2, 3, 4, 5];

function add(accumulator, currentValue, currentIndex, array) {
    return accumulator + currentValue;
}

function minus(accumulator, currentValue, currentIndex, array) {
    return accumulator - currentValue;
}

arr.reduce(add); // 15
arr.reduceRight(minus); // -5

[].reduce(add, 2); // 2
[].reduce(); // TypeError
```

## 21.arr.forEach()

**Array.prototype.forEach(callback[, thisArg])：** 对数组的每一项**有效值** 按升序执行一次callback，那些已删除或者未初始化的项将被跳过，返回``undefined`` 。forEach方法在循环是**不能被中止或跳出** 。

- callback：数组中每个元素执行的函数，该函数有三个参数：当前元素值、当前元素索引、原数组。
- thisArg：可选。执行callback时的this指向。

```javascript
var arr = ["a", "b", "c", , "d"];

function consoleElements(val, index, array) {
    console.log("a[" + index + "] = " + val);
}

arr.forEach(consoleElements); // undefined
// "a[0] = a"
// "a[1] = b"
// "a[2] = c"
// "a[4] = d"
```

## 22.arr.map()

**Array.prototype.map(callback[, thisArg])：** 对原数组的每一项**有效值** 都按顺序调用一次callback函数，并返回一个新数组，新数组的每一项为callback函数每一次的返回值。

- callback：数组中每个元素执行的函数，该函数有三个参数：当前元素值、当前元素索引、原数组。
- thisArg：可选。执行callback时的this指向。

```javascript
var arr = [1, 2, 3, 4, 5, "a", , undefined, null];
var multi = 2;
var obj = {
    multi: 3
}

function multiple(val, index, arr) {
    return val * this.multi;
}

arr.map(multiple); // [2, 4, 6, 8, 10, NaN, undefined, NaN, 0]
arr.map(multiple, obj); // [3, 6, 9, 12, 15, NaN, undefined, NaN, 0]
```

map()应用于某些类数组对象上：

```javascript
var arg = (function() {return arguments}) (1, 2, 3, 4)
var obj = {
    0: 1,
  	1: 2,
  	a: "a",
  	b: "b",
  	length: 2
}

function double(val, index, arr) {
    return val * 2;
}
function upper(val, index, arr) {
    return val.toUpperCase();
}

Array.prototype.map.call(arg, double); // [2, 4, 6, 8]
Array.prototype.map.call(obj, double); // [2, 4]
Array.prototype.map.call("abcdef", upper)
```

## 23.arr.sort()

**Array.prototype.sort(compareFunction)：** 在适当的位置对数组中的元素进行排序，并返回排序后的数组。默认排序是根据字符串的Unicode码点。

- compareFunction：可选。指定按某种顺序排序的的函数。如果省略，则按照字符串的Unicode码点排序。该方法有两个参数，a和b即将要被比较的元素：
  - 如果compareFunction(a ,b)的返回值小于0，那么a排列在b之前；
  - 如果compareFunction(a, b)的返回值等于0，那么a和b的相对位置不变；
  - 如果compareFunction(a, b)的返回值大于0，那么a排在b之后；

```javascript
var arr = [1011, 1101, 111]
function ascending(a, b) {
    return a - b;
}
function descending(a, b) {
    return b - a;
}

arr.sort(); // [1011, 1101, 111]
arr.sort(ascending); // [111, 1011, 1101]
arr.sort(descending); // [1101, 1011, 111]
```

## 24.arr.filter()

**Array.prototype.filter(callback, thisArg)：** 返回一个由经过callback测试的元素所组成的新数组。不改变原数组。

- callback：测试数组的每个元素的函数，该函数包含三个参数：元素值、元素索引、原数组。返回``true`` 表示通过测试，``false`` 表示未通过。

```javascript
var arr = [1, 2, 5, 6, 9];
var num = 2;
var obj = {
    num: 3
}
function filter(val, index, arr) {
    return val % this.num === 0;
}

arr.filter(filter); // [2, 6]
arr.filter(filter, obj); // [6, 9]
```

## 25.arr.toLocaleString()

**Array.prototype.toLocaleString()：** 返回一个字符串表示数组中的元素。数组中的元素使用各自的toLocaleString()方法转为字符串，这些字符串将使用一个特定语言环境的字符串隔开。

```javascript
var date = new Date();
var obj = {
    a: "a"
}
var arr = [1, "foo", date, obj];
arr.toLocaleString(); // "1,foo,9/4/2017, 11:49:09 AM,[object Object]"
```







1. 无参数时，返回一个空数组。
2. 单个
3. 单个合法正整数参数，返回一个``length属性`` 为该合法正整数的数组，且每个位置都为空值。
4. 单个非合法正整数的**数值** 参数，返回``RangeError`` 错误。
5. 单个