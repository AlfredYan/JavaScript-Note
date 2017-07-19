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
"isFinite" √
"isInteger" √
"isNaN" √
"isSafeInteger" √
"length" ×
"name" ×
"parseFloat" √
"parseInt" √
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

**使用方法：** 

1. parseInt()方法会将第一个参数转换为字符串，随后解析它，并返回一个数字或NaN。如果不是NaN，则将返回值作为指定基数的一个数值。
2. 在解析过程中，如果遇到不属于这个基数中的字符，则立即结束解析（忽略后面的字符），并返回已解析的整数。当字符串的第一个字符就无法解析时，会返回NaN。
3. 如果没有指定基数，在字符串以``0x`` 或``0X`` 开头，则以16(十六进制)作为基数；其他情况下都默认以10作为基数。当基数为``0 、undefined、null`` 时会自动忽略。

```javascript
parseInt("123"); // 123
parseInt("abc"); // NaN
parseInt("1a2b3c"); // 1
parseInt("0x12"); // 18
parseInt("abc", 20); // 4232

// 会先将0x10转为16进制数，即0x10 -> 17。再将17作为36进制数进行解析。
parseInt(0x10, 36); // 42
parseInt("17", 36); // 42
```

**Number.parseFloat(string)：** 解析一个字符串，并返回一个浮点数。

- string：将要被解析为浮点数的字符串。

**使用方法：** 

1. 在解析过程中如果遇到非正负号(+/-)，数字，小数点或科学计数法中的指数字符(e/E)，则会忽略其之后的字符，并返回当前的解析结果。当字符串的第一个字符就无法解析时，会返回NaN。
2. parseFloat()方法可以解析``Infinity`` 和``-Infinity`` 。

```javascript
parseFloat("314e-2"); // 3.14
parseFloat("123.4abd"); // 123.4
parseFloat("E1234"); // NaN
parseFloat(Infinity); // Infinity
```

## 6.Number.prototype对象

Number.prototype对象主要包含以下方法：

1. **numObj.toString([radix])：** 讲一个数值根据基数(进制)转为一个字符串。

   - radix：可选。用于表示数值的基数(2~36)，默认为10。

   ```javascript
   (20).toString(); // "20"
   (-20).toString(); // "-20"
   (-20).toString(20); // "-10"

   (-0xff).toString(2); // "-11111111"
   (0xff)["toString"](2); // "11111111"
   ```

2. **numObj.valueof()：** 返回一个被Number对象包装的原始值。

   ```javascript
   var num = new Number(10);

   num.valueOf(); 10// 
   ```

3. **numObj.toLocaleString(locale[, options])：** 返回数值在特定语言环境下所表示的字符串。（[参考](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Number/toLocaleString) ）

   ```javascript
   var number = 123456.789;
   number.toLocaleString('en-US', { style: 'currency', currency: 'USD' }) // "$123,456.79"
   ```

4. **numObj.toFixed(digits)：** 设定某数值小数点后所保留的位数。

   - digits：小数点后数字的个数(0~20)，保留位数时会四舍五入，默认为0。

   ```javascript
   var number = 123456.789;
   number.toFixed(); // "123457"
   number.toFixed(2); // "123456.79"
   number.toFixed(5); // "123456.78900"

   // 负数不会返回字符串
   (-1.23).toFixed(1); // -1.2
   ```

5. **numObj.toExponential(fractionDigits)：** 以科学计数法返回该数值的字符串表达式。

   - fractionDigits：可选，指定小数点后保留几位有效数字，会四舍五入，默认会尽可能多的保留。

   ```javascript
   var number = 123.4567;
   number.toExponential(); // "1.234567e+2"
   number.toExponential(2); // "1.23e+2"
   number.toExponential(6); // "1.23457e+2"
   ```

6. **numObj.toPrecision(precision)：** 以指定的精度返回该数值的字符串表达式

   - precision：用来指定有效位数的整数。

   ```javascript
   var number = 123.4567;
   number.toPrecision(); // "123.4567"
   number.toPrecision(6); // "123.457"
   number.toPrecision(3); // "123"
   number.toPrecision(2); // "1.2e+2"
   ```

   ​

