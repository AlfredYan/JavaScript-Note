# JavaScript原型与原型链

## 1.prototype属性

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

## 2.对象的[[Prototype]]属性

每一个JavaScript对象(null除外)都有一个属性，叫\_\_proto\_\_ ，这个属性用于指向该对象的原型，即该对象的原型链。

\_\_proto\_\_ 是内部[[Prototype]]的getter和setter方法，建议使用**Object.getPrototypeOf(obj)** 获取。

```javascript
function Student(){
  
}
var studentOne = new Student();
console.log(studentOne.__proto__ === Student.prototype); // true
```

## 3.constructor属性

每一个原型对象（prototype）都有一个constructor属性指向关联的构造函数。但是constructor并不表示被构造，而且它是一个可以被修改的属性。

```javascript
function Student(){
  
}
console.log(Student === Student.prototype.constructor); // true

var studentOne = new Student()
//由于studentOne中没有constructor，则去原型查找，所以实际是Student.prototype.constructor
console.log(studentOne.constructor === Student); // true
```

## 4.获取实例属性过程

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

## 5.instanceof运算符

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

## 6.new操作符构造调用的具体操作

```javascript
function Student(){ }

// 以下4步等价于：var studentOne = new Student();
var obj = new Object(); // 创建一个新的对象
obj.__proto__ = Student.prototype;// 将新对象的原型链指向构造函数的原型
var R = Student.call(obj); // 将this指向obj，将构造函数中的属性、方法绑定到obj

// 如果R（Student.call(obj)的返回值）为对象，则返回R，否则返回obj
if(typeof R === 'object') {
  var studentOne = R;
} else {
  var studentOne = obj;
}
```

## 7.对象原型链关系图

![JavaScript原型关系图 ](https://raw.githubusercontent.com/AlfredYan/JavaScript-Note/master/images/JavaScript%E5%8E%9F%E5%9E%8B%E5%85%B3%E7%B3%BB%E5%9B%BE.png)

图中蓝色所组成的链状结构即为**原型链** 。

## 8.JavaScript的面向对象编程

面向对象编程会将数据以及它相关的行为打包起来，形成一个对象。面向对象有三大特性：封装、继承和多态。JS是基于对象的语言，但JS中没有“类”的概念，它又不是真正的面向对象编程语言。

- **封装** 

  1. 构造函数模式：实际是一个普通函数，但在内部使用了``this`` 变量。对“构造函数”使用new运算符，会生成实例，并且this变量会绑定到实例对象上。

     ```javascript
     function Student(name, age) {
       this.name = name;
       this.age = age;
     }

     var student1 = new Student("Yan", 23);
     console.log(student1.name); // Yan
     console.log(student1.age); // 23
     ```

     当有公共属性或不变的属性时，使用构造函数模式会存在浪费内存的问题。因为每一次生成一个实例，都会重复创建不变的属性，造成内存的浪费。

     ```javascript
     function Student(name, age) {
       this.name = name;
       this.age = age;
       this.school = "Collage";
       this.showSchool = function() {console.log("学校")}
     }

     var student1 = new Student("Yan", 23);
     var student2 = new Student("XU", 20);
     console.log(student1.school); // Collage
     console.log(student2.school); // Collage
     console.log(student1.showSchool === student2.showSchool); // false
     ```

  2. Prototype模式：将那些公共属性或不变的属性直接定义到构造函数的prototype对象上，能有效解决内存的浪费问题。并且会被构造函数的实例继承。

     ```javascript
     function Student(name, age) {
       this.name = name;
       this.age = age;
     }
     Student.prototype.school = "Collage";
     Student.prototype.showSchool = function() {console.log("学校")}

     var student1 = new Student("Yan", 23);
     var student2 = new Student("XU", 20);
     console.log(student1.school); // Collage
     console.log(student2.school); // Collage
     console.log(student1.showSchool === student2.showSchool); // true
     console.log(Student.prototype.isPrototypeOf(student1)) // true
     console.log(Student.prototype.isPrototypeOf(student2)) // true
     ```

- **继承** 

  1. 构造函数绑定：使用call或apply方法，将父对象构造函数绑定到子对象

     ```javascript
     function Child(a){
       Parents.apply(this, arguments);
       this.a = a;
     }
     ```

  2. prototype模式继承 （[参考](http://www.ruanyifeng.com/blog/2010/05/object-oriented_javascript_inheritance_continued.html) ）

     ```javascript
     function extend(Child, Parent) {
       
       // 使用一个空对象作为中介(空对象几乎不占内存)
       // 这样在修改Child的prototype对象时不会影响到Parent的prototype属性
       var F = function() {};
       F.prototype = Parent.prototype;
       Child.prototype = new F();
       
       // 手动修复Child的constructor的指向
       Child.prototype.constructor = Child;
       // 直接指向父对象的prototype，为了实现继承的完备性，属于备用性质
       Child.uber = Parent.prototype;
     }

     // 使用方法
     extend(Child, Parent);
     var child1 = new Child();
     ```

  3. 拷贝继承（[浅复制&深复制](https://github.com/AlfredYan/JavaScript-Note/blob/master/notes/%E5%AF%B9%E8%B1%A1.md#9对象复制) ）

  4. Object.create(proto)实现继承

     ```javascript
     // 部分实现create方法
     Object.create = function(o) {
       function F() {}
       F.prototype = o;
       return new F();
     }
     ```

## 9.行为委托

在基于类的设计模式中，每创建一个对象，都会得到到一个新的副本。在JavaScript中并没有类的概念，而且JS的对象机制并不会自动执行复制行为，而是通过[[prototype]]将对象之间关联起来。所以面向类的设计模式并不适合JS，面向委托的设计更加适合JS，这种编码风格称为“对象关联”（OLOO：Objects Linked to Other Objects）。

委托行为意味着在对象在找到属性或者方法时，会把这个请求委托给与他关联的对象。在委托行为设计模式中，尽量在[[prototype]]链的不同级别上使用不同的命名。

```javascript
var Task = {
  setID: function(ID) {this.id = ID},
  outputID: function() {console.log(this.id)}
}

// 让o委托Task
var o = Object.create(Task);

// 不要在使用setID命名，使用更有描述性的方法名
o.prepareTask = function(ID, label) {
  this.setID(ID);
  this.label = label;
};

o.outputTask = function() {
  this.outputID();
  console.log(this.label);
};

o.prepareTask(123, "Hello");
o.outputTask(); // 123 Hello
```

## 10.总结

1. prototype只存在于函数中，是一个指针，指向该函数所创建的对象的原型。其中包含一个constructor和

   \_\_proto\_\_ (实际来自于Object.prototype)和一些独有的方法。

2. JavaScript每一个对象中都有一个\_\_proto\_\_ 属性，它指向该对象的原型。

3. 读取实例的属性时，如未找到，则去实例的原型查找，直到原型链的顶端。

4. Function.\_\_proto\_\_ === Function.prototype

5. Object.prototype的[[prototype]]为null，即对象原型链的顶端。

6. JS中没有类的概念，而且对象之间是通过[[prototype]]关联起来的，所以委托行为的设计模式更加适合JS