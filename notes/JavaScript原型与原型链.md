# JavaScript原型与原型链

## 构造函数与新建实例

创建构造函数Student， 使用new创建一个实例对象student。

```javascript
function Student(){
  
}
var student = new Student();
```

## prototype属性

只有函数中存在prototype属性(指针)，其**指向一个对象** ，这个对象就是调用该构造函数所创建的**实例(对象)的原型** 。

**对象的原型**：每一个JavaScript对象在创建时，会与另一个对象关联，这个被关联的对象就是原型。在两个对象关联后，新创建的对象便可以访问被关联对象的属性与方法(继承)。

在下面的例子中，studentOne与studentTwo的原型就是Student的prototype属性所指向的对象。

```javascript
function Student(){
  
}
Student.prototype.name = 'Alfred';
var studentOne = new Student();
var studentTwo = new Student();
console.log(studentOne.name); // Alfred
console.log(studentTwo.name); // Alfred
```

## 对象的_\_proto\_\_属性

每一个JavaScript对象(null除外)都有一个属性，叫\_\_proto\_\_ ，这个属性用于指向该对象的原型。

\_\_proto\_\_ 是内部[[Prototype]]的getter和setter方法，建议使用Object.getPrototypeOf(obj)获取。

```javascript
function Student(){
  
}
var studentOne = new Student();
console.log(studentOne.__proto__ === Student.prototype); // true
```

## constructor

每一个原型对象都有一个constructor属性指向关联的构造函数。

```javascript
function Student(){
  
}
console.log(Student === Student.prototype.constructor); // true

var studentOne = new Student()
//由于studentOne中没有constructor，则去原型查找，所以实际是Student.prototype.constructor
console.log(studentOne.constructor === Student); // true
```

## 实例与原型

当读取实例的属性时，如果未找到，就会去实例的原型中查找该属性，如果还是未找到，就会去原型的原型查找，直到最顶层(原型链的源头：Object.prototype)。

Object.prototype的原型为null。

```javascript
function Student(){
  
}
Student.prototype.name = "No Name";

var studentOne = new Student();
studentOne.name = "Alfred";
console.log(studentOne.name); // Alfred

delete studentOne.name;
console.log(studentOne.name); // No Name

console.log(Object.prototype.__proto__ === null); // true
```

## JavaScript中"万物皆对象"

Number、String、Boolean、Symbol和Function的原型都为Object.prototype。

```javascript
console.log(Boolean.prototype.__proto__ === Object.prototype); // true
console.log(String.prototype.__proto__ === Object.prototype); // true
console.log(Number.prototype.__proto__ === Object.prototype); // true
console.log(Symbol.prototype.__proto__ === Object.prototype); // true
console.log(Function.prototype.__proto__ === Object.prototype); // true

console.log(Boolean.__proto__ === Function.prototype); // true
console.log(String.__proto__ === Function.prototype); // true
console.log(Number.__proto__ === Function.prototype); // true
console.log(Symbol.__proto__ === Function.prototype); // true
console.log(Function.__proto__ === Function.prototype); // true
```

## instanceof运算符

instanceof运算符用来检测一个对象在其原型链上是否存在一个构造函数的prototype属性

```javascript
object instanceof constructor
// object: 要检测的对象
// constructor: 某个构造函数
// object.__proto__ === constructor.prototype

function Student(){
  
}
var studentOne = new Student();
// studentOne.__proto__ === Student.prototype
console.log(studentOne instanceof Student); // true

console.log(Boolean instanceof Function); // true
console.log(Boolean instanceof Boolean); // false
console.log(String instanceof Function); // true
console.log(Number instanceof Function); // true
console.log(Symbol instanceof Function); // true
console.log(Function instanceof Function); // true
```

## new的具体操作

```javascript
function Student(){
  
}

// 以下4步等价于：var studentOne = new Student();
var obj = new Object(); // 创建一个新的对象
obj.__proto__ = Student.prototype;// 将新对象的原型链指向构造函数的原型
Student.call(obj); // 将this指向obj，将构造函数中的属性、方法绑定到obj
var studentOne = obj; // 将obj返回给studentOne
```

## 关系图

![JavaScript原型关系图 ](https://raw.githubusercontent.com/AlfredYan/JavaScript-Note/master/images/JavaScript%E5%8E%9F%E5%9E%8B%E5%85%B3%E7%B3%BB%E5%9B%BE.png)

图中蓝色所组成的链状结构即为**原型链** 。

## 总结

1. prototype只存在于函数中，是一个指针，指向该函数所创建的对象的原型。其中包含一个constructor和

   \_\_proto\_\_ (实际来自于Object.prototype)和一些独有的方法。

2. JavaScript每一个对象中都有一个\_\_proto\_\_ 属性，它指向该对象的原型。

3. 读取实例的属性时，如未找到，则去实例的原型查找，知道原型链的顶端。

4. Function.\_\_proto\_\_ === Function.prototype

5. Object.prototype.\_\_proto\_\_ === null