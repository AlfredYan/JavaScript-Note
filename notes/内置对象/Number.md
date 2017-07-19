# Number

简介：学习Number对象的属性、方法和实例方法。（[参考](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Number) ） 

Number对象中的属性与方法：

```javascript
Object.getOwnPropertyNames(Number).sort().forEach(function(val) { console.log(val); });

"EPSILON" √
"MAX_SAFE_INTEGER" √
"MAX_VALUE" √
"MIN_SAFE_INTEGER" √
"MIN_VALUE" √
"NEGATIVE_INFINITY" √
"NaN" √
"POSITIVE_INFINITY" √
"arguments" ×
"caller" ×
"isFinite"
"isInteger"
"isNaN"
"isSafeInteger"
"length" ×
"name" ×
"parseFloat"
"parseInt"
"prototype"
```

## 1.简介

Number对象是数值的包装对象。

- 作为构造函数，可以生成值为数值的对象

  ```javascript
  var n = new Number(1);
  typeof n; "object"
  ```

- 作为工具函数，可以将任意类型转换为数值

  ```javascript
  Number("1"); // 1
  Number(true); // 1
  Number("a"); // NaN
  ```

## 2.Number对象的属性

- **Number.EPSILON：** 1和大于1的最小值的差值，接近于2<sup>-52</sup>，常用于调整精度的误差范围值。

- **Number.MAX_VALUE：** JavaScript中所能表示的最大正数。约为：1.7976931348623157e+308

- **Number.MIN_VALUE：** JavaScript中所能表示的最小正数。约为：5e-324

- **Number.MAX_SAFE_INTEGER：** JavaScript所能精确表示的最大整数。为：2<sup>53</sup>-1，

  即9007199254740991

- **Number.MIN_SAFE_INTEGER：** JavaScript所能精确表示的最小整数。为：-(2<sup>53</sup>-1)，

  即-9007199254740991

- **Number.NEGATIVE_INFINITY：** 表示负无穷大，指向``Infinity`` 。

- **Number.POSITIVE_INFINITY：** 表示正无穷大，指向``-Infinity`` 。

- **Number.NaN：** 表示非数字（Not a Number），指向``NaN`` 。

## 3.Number.isInteger() & Number.isSafeInteger()

**Number.isInteger(val)：** 检测传入的参数是否为整数。

- val：将要被检测的值

**Number.isSafeInteger(val)：** 判断传入的参数是否为一个安全的整数，即该整数是否在-(2<sup>53</sup>-1)至2<sup>53</sup>-1之间。

- val：将要被检测的值

```javascript
Number.isInteger(1); // true
Number.isInteger(1.1); // false
Number.isInteger(Math.pow(2, 53)); // true

Number.isSafeInteger(Math.pow(2, 53) - 1); // true
Number.isSafeInteger(Math.pow(2, 53)); // false
Number.isSafeInteger(-Math.pow(2, 53) + 1); // true
Number.isSafeInteger(-Math.pow(2, 53)); // false
```

## 4.Number.isFinite() & Number.isNaN()

**Number.isFinite(val)：** 检测传入的参数是否为有穷数，并且数据类型是否为Number。

- val：将要被检测的值

**Number.isNaN(val)：** 检测传入的参数是否为NaN，并且数据类型是否为Number。

- val：将要被检测的值

```javascript
Number.isFinite(10); // true
Number.isFinite("10"); // false
Number.isFinite(Infinity); // false
Number.isFinite(-Infinity); // false
Number.isFinite(NaN); // false

Number.isNaN(NaN); // true
Number.isNaN(0 / 0); // true
Number.isNaN("NaN"); // false
```

## 5.Number.parseInt() & Number.parseFloat()

**Number.parseInt(string[, radix])：** 根据给定的进制数(radix)把一个字符串解析为整数。

- string：将要被解析的值，如果不是string，则会自动将其转换为字符串。字符串开头的空白符会被忽略。
- radix：可选。是一个2~36之间的整数，表示需要被解析字符串的基数(进制)，通常默认为10。

