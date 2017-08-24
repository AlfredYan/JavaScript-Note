# String

简介：学习String对象的属性、方法和实例方法。

String对象中属性与方法：

```javascript
Object.getOwnPropertyNames(String).sort().forEach(function(val) { console.log(val); });

"arguments" ×
"caller" ×
"fromCharCode" √
"fromCodePoint" √
"length" ×
"name" ×
"prototype" √
"raw" √
```

## 1.简介

String对象是字符串的包装对象，即当做**构造函数** 使用。使用valueOf()方法能返回它所包装的字符串。

```javascript
var s1 = 'abc';
var s2 = new String('abc');

typeof s1; // string
typeof s2; // object

s2.valueOf(); // abc
```

在当做**工具方法** 使用时，可将任意类型的值转为字符串。

```javascript
String(true); // "true"
String(1); // "1"
```

## 2.String.fromCharCode() & Stirng.fromCodePoint()

**String.fromCharCode(num1, …, numN)：** 返回使用指定的Unicode值序列创建的字符串。

- num1, …, numN：一组序列数字，表示Unicode值。（不支持Unicode大于0xFFFF的字符，如果要表示大于0xFFFF的值，则要将其拆成2个字符表示([转换公式](https://github.com/AlfredYan/JavaScript-Note/blob/master/notes/%E5%AD%97%E7%AC%A6%E4%B8%B2.md#4字符集) )）

```javascript
String.fromCharCode(65, 66, 67); // "ABC"
String.fromCharCode(0x10041); // "A" (返回的是0x0041的值)
String.fromCharCode(0xD800, 0xDC41); // "𐁁" (0x10041对应的值) 
```

**String.fromCodePoint(num1[, …[, numN]])：** 返回指定的代码点序列创建的字符串。

- num1[, …[, numN]]：一串Unicode编码。当传入无效的Unicode编码时，会抛出``RangeError`` 。

```javascript
String.fromCodePoint(0x10041) // "𐁁"
String.fromCodePoint("a") // RangeError
```

## 3.String.raw()

**String.raw(callSite, …substitutions)：** 获取一个模板字符串的原始字面量值。

- callSite：模板字符串的“调用点对象”。
- …substitutions：表示任意个内插表达式对应的值。

```javascript
String.raw `Hi\n!`; // "Hi\n"
String.raw({raw: "test"}, 0, 1, 2); // "t0e1s2t"
```

## 4.String.prototype对象

String的实例方法：

```javascript
Object.getOwnPropertyNames(String.prototype).sort().forEach(function(val) { console.log(val); });

"anchor" ☹️
"big" ☹️
"blink" ☹️
"bold" ☹️
"charAt" √
"charCodeAt" √
"codePointAt" √
"concat" √
"constructor" ×
"endsWith" √
"fixed" ☹️
"fontcolor" ☹️
"fontsize" ☹️
"includes" √
"indexOf" √
"italics" ☹️
"lastIndexOf" √
"length" ×
"link" √
"localeCompare" √
"match" √
"normalize" √
"padEnd" √
"padStart" √
"repeat" √
"replace" √
"search" √
"slice" √
"small" ☹️
"split" √
"startsWith" √
"strike" ☹️
"sub" ☹️
"substr" √
"substring" √
"sup" ☹️
"toLocaleLowerCase" √
"toLocaleUpperCase" √
"toLowerCase" √
"toString" √
"toUpperCase" √
"trim" √
"trimLeft" √
"trimRight" √
"valueOf" √
```

## 5.strObj.toString() & strObj.valueOf()

**String.prototype.toString()：** 返回指定对象的字符串形式。其覆盖了[Object.prototype.toString()](https://github.com/AlfredYan/JavaScript-Note/blob/master/notes/%E5%86%85%E7%BD%AE%E5%AF%B9%E8%B1%A1/Object.md#13objectprototype对象) 。

**String.prototype.valueOf()：** 返回String对象的原始值。

```javascript
var str = new String("Hello World");

str.toString(); // "Hello World"
str.valueOf(); // "Hello World"
```

## 6.strObj.charAt()

**String.prototype.charAt(index)：** 从一个字符串中返回指定的字符。

- index：0 ~ (length-1)。如果未提供索引，则使用0。当索引超出范围时，返回空字符串。

```javascript
var str = "world"

str.charAt(); // "w"
str.charAt(0); // "w"
str.charAt(1); // "o"
str.charAt(10); // ""
```

## 7.strObj.charCodeAt() & strObj.codePointAt()

**String.prototype.charCodeAt(index)：** 返回一个表示给定索引出字符的Unicode值，为在0到65535之间的整数。如果超出范围，则返回``NaN`` 。

- index：一个在0到字符串长度之间整数。默认为0。

```javascript
var str = "world";
str.charCodeAt(); // 119
str.charCodeAt(1); // 111
str.charCodeAt(5); // NaN

'\uD800\uDC00'.codePointAt(); // 55296
```

**String.prototype.codePointAt(index)：** 返回一个Unicode编码点值的非负数。

- index：字符串中需要转码的元素的位置。如果该位置没有元素，则返回``undefined`` 。

```javascript
var str = "world";
str.codePointAt(); // 119
str.codePointAt(1); // 111
str.codePointAt(5); // undefined

'\uD800\uDC00'.codePointAt(); // 65536
```

## 8.strObj.concat()

**String.prototype.concat(str1, str2[, …, strN])：** 将一个或多个字符串与原字符串拼接，并返回一个新的字符串。

- str1, str2[, …, strN]：和原字符串拼接的字符串。如果不为字符串，则会先转换为字符串再拼接。

```javascript
var str = "	World";
var str1 = "Str1";
var str2 = "Str2";
str.concat(str1, str2); // "WorldStr1Str2"

var one = 1;
var two = 2;
var three = "3";
"".concat(one, two, three); // "123"
one + two + three; // "33"
```

``+`` 运算符在2个运算数都为数值时，不会转换类型。当运算数中有字符串时，会返回一个拼接后的字符串。

## 9.strObj.padStart() & strObj.padEnd()

**String.prototype.padStart(targetLength[, padString])：** 用一个字符串填充当前字符串，在当前字符串头部不断填充，直到到达目标长度。

- targetLength：当前字符串要填充到的目标长度，如果当前字符串到达了该长度，则什么也不做，返回原字符串。
- padString：可选。填充字符串。默认值为``""`` 。

**String.prototype.padEnd(targetLength[, padString])：** 用一个字符串填充当前字符串，在当前字符串尾部不断填充，直到到达目标长度。

- targetLength：当前字符串要填充到的目标长度，如果当前字符串到达了该长度，则什么也不做，返回原字符串。
- padString：可选。填充字符串。默认值为``""`` 。

```javascript
var str = "Hello World";

str.padStart(15); // "    Hello World"
str.padStart(15, "start"); // "starHello World"
str.padEnd(15); // "Hello World    "
str.padEnd(15, "end"); // "Hello Worldende"
```

## 10.strObj.repeat()

**String.prototype.repeat(count)：** 连接指定数量的字符串副本，并返回。

- count：**0~正无穷大** 之间的整数，表示要重复的次数。

```javascript
var str = "abc";

str.repeat(0); // ""
str.repeat(3); // "abcabcabc"
str.repeat(3.5); // "abcabcabc"
str.repeat(-2); // RangeError
```

## 11.strObj.startsWith() & strObj.endsWith()

**String.prototype.startsWith(searchString [, position])：** 判断当前字符串是否以另一个给定的子字符串”开头“，返回true或false。

- searchString：要搜索的子字符串。
- position：在strObj中搜索searchString的开始位置，默认为0。

**String.prototype.endsWith(searchString [, position])：** 判断当前字符串是否以另一个给定的字符串”结尾“的，返回true或false。

- searchString：要搜索的子字符串。
- position：在strObj中搜索searchString的结束位置，默认为strObj.length。

```javascript
var str = "To be, or not to be, that is the question.";

str.startsWith("To be"); // true
str.startsWith("not to be"); // false
str.startsWith("not to be", 10); // true

str.endsWith("questions."); // true
str.endsWith("not to be"); // false
str.endsWith("not to be", 19); // true
```

## 12.strObj.indexOf() & strObj.lastIndexOf()

**String.prototype.indexOf(searchValue [, fromIndex])：** 返回第一次出现searchValue的索引值，从fromIndex开始进行搜索。如果未找到该值，则返回``-1`` 。

- searchValue：字符串，表示要被查找的值。
- fromIndex：任意整数，表示开始查找的位置，默认为0。如果小于0，则查找整个字符串；如果大于str.length，则返回-1，除非被查找的字符串是一个空字符串，此时返回str.length。

**String.prototype.lastIndexOf(searchValue [, fromIndex])：** 返回最后一次出现searchValue的索引值，从该字符串的后面向前查找，从fromIndex开始，如果没找则返回``-1`` 。

- searchVale：字符串，表示要被查找的值。
- fromIndex：任意整数，表示开始查找的位置，默认值为str.length。如果为负值，则被看做0；如果大于str.length，则被看做str.length。

```javascript
var str = "Hello World";

str.indexOf("Hello"); // 0
str.indexOf("hello"); // -1
str.indexOf("World", 3); // 6
str.indexOf("", 10); // 10
str.indexOf("", 11); // 11

str.lastIndexOf("o"); // 7
str.lastIndexOf("o", 6); // 4
```

## 13.strObj.includes()

**String.prototype.includes(searchString [, position])：** 判断一个字符串中是否包含在另一个字符串中。返回true或false。

- searchString：子字符串，将要被查找的字符串。
- position：从当前字符串的哪个索引开始查找子字符串，默认为0。

```javascript
var str = "Hello World";

str.includes('Hello'); // true
str.includes('o', 7); // true
str.includes('o', 8); // false
```

## 14.strObj.localeCompare()

**String.prototype.localeCompare(compareString) ：** 比较两个字符串的大小，返回一个整数。如果小于0，表示第一个字符串小于第二个字符串；如果等于0，表示两者相等；如果大于0，表示第一个字符串大于第二个字符串。该方法会考虑自然语言的排序情况（比如：B排在a的前面）。

- compareString：用来比较的字符串（第二个字符串）。

```javascript
var Banana = "Banana";

Banana.localeCompare("apple"); // 1
Banana.localeCompare("Peach"); // -1
Banana.localeCompare("banana"); // 1
Banana.localeCompare("Banana"); // 0
```

## 15.strObj.match() & strObj.search() & strObj.replace()

**String.prototype.match(regexp)：** 判断原字符串中是否匹配某个子字符串。如果匹配成功，返回一个包含整个匹配结果的**数组** ；如果没有匹配项，则返回``null`` 。

- regexp：一个正则表达式对象

如果正则表达式没有g标志(global)，返回的数组中包含``input`` 和``index`` 两个属性，分别表示**原始字符串** 和**匹配字符串开始的位置** 。

如果正则表达式有g标志(global)，返回的数组中含有所有**匹配的子字符串** ，但不会返回``index`` 和``input`` 属性。没有匹配到则返回``null`` 。

```javascript
var str = "Abcdabcd";

str.match(/ab/); // ["ab", index: 4, input: "Abcdabcd"]
str.match(/ab/gi); // ["Ab", "ab"]
```

**String.prototype.search(regexp)：** 判断原字符串中是否包含某子字符串。如果匹配成功，返回首次匹配项的索引；如果匹配失败，返回-1。

- regexp：一个正则表达式对象。

```javascript
var str = "abcdabcd";

str.search(/ab/); // 0
```

**String.prototype.replace(regexp/substr, newSubStr/function)：** 返回由替换值(**newSubStr** )替换一些或所有匹配值后的新字符串。

- regexp：一个正则表达式，该正则表达式所匹配的内容会被第二个参数的值替换。
- substr：一个字符串，仅仅替换第一个匹配。
- newSubStr(replacement)：用于替换第一个参数在原字符串中所匹配的字符串。
- function(replacement)：用于创建新子字符串的函数，该函数的返回值将替换第一个参数所匹配的的结果。

```javascript
var str = "Apples are round, and apples are juicy."

str.replace("apple", "orange"); // "Apples are round, and oranges are juicy."
str.replace(/apple/, "orange"); // "Apples are round, and oranges are juicy."
str.replace(/apple/gi, "orange"); // "oranges are round, and oranges are juicy."
```

交换字符串中的两个单词

```javascript
var str = "Snow John";

str.replace(/(\w+)\s(\w+)/, "$2 $1"); // "John Snow"
```

自定义函数修改匹配的字符

```javascript
var str = "marginTop, borderTop";

function upperToLower(match) {
  return "-" + match.toLowerCase();
}

str.replace(/[A-Z]/g, upperToLower); // "margin-top, border-top"
```

## 16.strObj.slice() & strObj.substr() & strObj.substring()

**String.prototype.slice(beginSlice[, endSlice])：** 提取字符串的一部分，并返回一个新的字符串。

- beginSlice：从该索引处开始提取原字符串中的字符。如果为负数，则被视为**字符串长度+beginSlice** 。
- endSlice：可选。在该索引处结束提取字符串(不含该位置)。如果省略该参数，则会一直提取到字符串的末尾。如果为负数，则被视为**字符串长度+endSlice** 。

```javascript
var str = "Hello World";

str.slice(0, 5); // "Hello"
str.slice(6); // "World"
str.slice(0, -6); // "Hello"

// 当第一个参数大于第二个参数时，返回一个空字符串。
str.slice(3, 1); // ""
```

**Stirng.prototype.substr(start[, length])：** 返回一个字符串从指定位置开始到指定字符数的字符。

- start：开始提取字符的位置。如果为负值，被视为**字符串长度+start** 。
- length：可选。提取的字符数。省略则表示一直提取到原字符串末尾。如果为负值，被自动转为0。

```javascript
var str = "Hello World";

str.substr(0, 5); // "Hello"
str.substr(6); // "World"
str.substr(-5); // "World"
str.substr(6, -1); // ""
```

**String.prototype.substring(indexStart[, indexEnd])：** 返回一个字符串在开始索引到结束索引之间的子集。

- indexStart：**0~字符串长度** 的整数。
- indexEnd：可选。**0~字符串长度** 的整数(不含该位置)。如果省略，则一直提取到原字符串末尾。

任意参数小于0，则被视为0；大于原字符串长度，则被视为原字符串长度。如果indexEnd小于indexStart，则自动调换两个参数的位置。

```javascript
var str = "Hello World";

str.subString(0, 5); // "Hello"
str.subString(5, 0); // "Hello"
str.subString(-1, 15); // "Hello World"
```

## 17.strObj.split()

**String.prototype.split([separator[, limit]])：** 将一个字符串对象分割为字符串数组。

- separator：指定用来分割原字符串的字符串。如果省略，返回整个字符串的数组形式。如果是一个**空字符串** ，则将原字符串中每个字符的数组形式返回。当分割符在原字符串的开头或结尾时，返回的数组的第一项或最后一项为一个空字符串。
- limit：一个整数，限定返回的分割片段数量。

```javascript
var str = "a,b,c";

str.split(); // ["a,b,c"]
str.split(","); // ["a", "b", "c"]
str.split(",", 2); // ["a", "b"]

// 分隔符在原字符串的开头或结尾
",b,c".split(","); // ["", "b", "c"]
"a,b,".split(","); // ["a", "b", ""]
```

## 18.strObj.toLowerCase() & strObj.toLocaleLowerCase() & strObj.toUpperCase() & strObj.toLocaleUpperCase()

**String.prototype.toLowerCase()：** 将调用字符串值转为小写形式，并返回一个新的字符串。

**String.prototype.toLocaleLowerCase()：** 将调用字符串值转为小写形式，同时适应宿主环境的当前区域设置。

**String.prototype.toUpperCase()：** 将调用字符串的值转为大写形式，并返回一个新的字符串。

**String.prototype.toLocaleUpperCase()：** 将调用字符串值转为大写形式，同时适应宿主环境的当前区域设置。

```javascript
var str = "Hello World";

str.toLowerCase(); // "hello world"
str.toLocaleLowerCase(); // "hello world"
str.toUpperCase(); // "HELLO WORLD"
str.toLocaleUpperCase(); // "HELLO WORLD"
```

## 19.strObj.trim() & strObj.trimLeft() & strObj.trimRight()

**String.prototype.trim()：** 将字符串两端的空白字符删除，并返回一个新的字符串。

**String.prototype.trimLeft()：** 将字符串左边的空白字符删除，并返回一个新的字符串。

**String.prototype.trimRight()：** 将字符串右边的空白字符删除，并返回一个新的字符串。

```javascript
var str = "  Hello world  ";

str.trim(); // "Hello world"
str.trimLeft(); // "Hello world  "
str.trimRight(); // "  Hello world"
```

## 20.strObj.normalize()

**String.prototype.normalize(form)：** 按照指定的一种Unicode正规形式将当前字符串正规化。（[参考](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/normalize) ）

- form：正规形式，一共有4种：NFC、NFD、NFKC、NFKD。

## 21.strObj.link()

**String.prototype.link(url)：** 创建一个``<a>`` HTML元素，用该字符串作为超链接的显示文本。

- url：能指定a标签的href属性的字符串。

```javascript
var text = "Google";
var url = "https://www.google.com/";

text.link(url); // "<a href=\"https://www.google.com/\">Google</a>"
```

