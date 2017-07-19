# JavaScript-Note
记录一些JavaScript的知识点

## [JavaScript作用域及闭包](https://github.com/AlfredYan/JavaScript-Note/blob/master/notes/JavaScript%E4%BD%9C%E7%94%A8%E5%9F%9F%E5%8F%8A%E9%97%AD%E5%8C%85.md) 

- [编译及变量查找方法](https://github.com/AlfredYan/JavaScript-Note/blob/master/notes/JavaScript%E4%BD%9C%E7%94%A8%E5%9F%9F%E5%8F%8A%E9%97%AD%E5%8C%85.md#1编译及变量查找方法) 
- [词法作用域](https://github.com/AlfredYan/JavaScript-Note/blob/master/notes/JavaScript%E4%BD%9C%E7%94%A8%E5%9F%9F%E5%8F%8A%E9%97%AD%E5%8C%85.md#2词法作用域) 
- [函数作用域](https://github.com/AlfredYan/JavaScript-Note/blob/master/notes/JavaScript%E4%BD%9C%E7%94%A8%E5%9F%9F%E5%8F%8A%E9%97%AD%E5%8C%85.md#3函数作用域) 
- [提升](https://github.com/AlfredYan/JavaScript-Note/blob/master/notes/JavaScript%E4%BD%9C%E7%94%A8%E5%9F%9F%E5%8F%8A%E9%97%AD%E5%8C%85.md#4提升) 
- [执行上下文栈](https://github.com/AlfredYan/JavaScript-Note/blob/master/notes/JavaScript%E4%BD%9C%E7%94%A8%E5%9F%9F%E5%8F%8A%E9%97%AD%E5%8C%85.md#5执行上下文栈) 
- [变量对象](https://github.com/AlfredYan/JavaScript-Note/blob/master/notes/JavaScript%E4%BD%9C%E7%94%A8%E5%9F%9F%E5%8F%8A%E9%97%AD%E5%8C%85.md#6变量对象) 
- [作用域链](https://github.com/AlfredYan/JavaScript-Note/blob/master/notes/JavaScript%E4%BD%9C%E7%94%A8%E5%9F%9F%E5%8F%8A%E9%97%AD%E5%8C%85.md#7作用域链) 
- [闭包](https://github.com/AlfredYan/JavaScript-Note/blob/master/notes/JavaScript%E4%BD%9C%E7%94%A8%E5%9F%9F%E5%8F%8A%E9%97%AD%E5%8C%85.md#8闭包) 
- [总结](https://github.com/AlfredYan/JavaScript-Note/blob/master/notes/JavaScript%E4%BD%9C%E7%94%A8%E5%9F%9F%E5%8F%8A%E9%97%AD%E5%8C%85.md#总结) 


## [this深入理解](https://github.com/AlfredYan/JavaScript-Note/blob/master/notes/this%E6%B7%B1%E5%85%A5%E7%90%86%E8%A7%A3.md) 

- [this基本概念](https://github.com/AlfredYan/JavaScript-Note/blob/master/notes/this%E6%B7%B1%E5%85%A5%E7%90%86%E8%A7%A3.md#1this基本概念) 
- [this绑定规则](https://github.com/AlfredYan/JavaScript-Note/blob/master/notes/this%E6%B7%B1%E5%85%A5%E7%90%86%E8%A7%A3.md#2this绑定规则) 
- [this词法（箭头函数）](https://github.com/AlfredYan/JavaScript-Note/blob/master/notes/this%E6%B7%B1%E5%85%A5%E7%90%86%E8%A7%A3.md#3this词法箭头函数) 
- [从ECMAScript规范了解this](https://github.com/AlfredYan/JavaScript-Note/blob/master/notes/this%E6%B7%B1%E5%85%A5%E7%90%86%E8%A7%A3.md#4从ecmascript规范了解this) 
- [总结](https://github.com/AlfredYan/JavaScript-Note/blob/master/notes/this%E6%B7%B1%E5%85%A5%E7%90%86%E8%A7%A3.md#5总结) 

## [JavaScript原型与原型链](https://github.com/AlfredYan/JavaScript-Note/blob/master/notes/JavaScript%E5%8E%9F%E5%9E%8B%E4%B8%8E%E5%8E%9F%E5%9E%8B%E9%93%BE.md)

- [prototype属性](https://github.com/AlfredYan/JavaScript-Note/blob/master/notes/JavaScript%E5%8E%9F%E5%9E%8B%E4%B8%8E%E5%8E%9F%E5%9E%8B%E9%93%BE.md#1prototype属性) 
- [原型链](https://github.com/AlfredYan/JavaScript-Note/blob/master/notes/JavaScript%E5%8E%9F%E5%9E%8B%E4%B8%8E%E5%8E%9F%E5%9E%8B%E9%93%BE.md#2对象的prototype属性)
- [constructor属性](https://github.com/AlfredYan/JavaScript-Note/blob/master/notes/JavaScript%E5%8E%9F%E5%9E%8B%E4%B8%8E%E5%8E%9F%E5%9E%8B%E9%93%BE.md#3constructor属性) 
- [获取实例属性过程](https://github.com/AlfredYan/JavaScript-Note/blob/master/notes/JavaScript%E5%8E%9F%E5%9E%8B%E4%B8%8E%E5%8E%9F%E5%9E%8B%E9%93%BE.md#4获取实例属性过程) 
- [instanceof运算符](https://github.com/AlfredYan/JavaScript-Note/blob/master/notes/JavaScript%E5%8E%9F%E5%9E%8B%E4%B8%8E%E5%8E%9F%E5%9E%8B%E9%93%BE.md#5instanceof运算符) 
- [new操作符构造调用的具体操作](https://github.com/AlfredYan/JavaScript-Note/blob/master/notes/JavaScript%E5%8E%9F%E5%9E%8B%E4%B8%8E%E5%8E%9F%E5%9E%8B%E9%93%BE.md#6new操作符构造调用的具体操作) 
- [对象原型链关系图](https://github.com/AlfredYan/JavaScript-Note/blob/master/notes/JavaScript%E5%8E%9F%E5%9E%8B%E4%B8%8E%E5%8E%9F%E5%9E%8B%E9%93%BE.md#7对象原型链关系图) 
- [JavaScript的面向对象编程](https://github.com/AlfredYan/JavaScript-Note/blob/master/notes/JavaScript%E5%8E%9F%E5%9E%8B%E4%B8%8E%E5%8E%9F%E5%9E%8B%E9%93%BE.md#8javascript的面向对象编程) 
- [行为委托](https://github.com/AlfredYan/JavaScript-Note/blob/master/notes/JavaScript%E5%8E%9F%E5%9E%8B%E4%B8%8E%E5%8E%9F%E5%9E%8B%E9%93%BE.md#9行为委托) 
- [总结](https://github.com/AlfredYan/JavaScript-Note/blob/master/notes/JavaScript%E5%8E%9F%E5%9E%8B%E4%B8%8E%E5%8E%9F%E5%9E%8B%E9%93%BE.md#10总结) 

## [数据类型](https://github.com/AlfredYan/JavaScript-Note/blob/master/notes/%E6%95%B0%E6%8D%AE%E7%B1%BB%E5%9E%8B.md)

- [数据类型概述](https://github.com/AlfredYan/JavaScript-Note/blob/master/notes/%E6%95%B0%E6%8D%AE%E7%B1%BB%E5%9E%8B.md#1数据类型概述) 
- [数据类型的判断](https://github.com/AlfredYan/JavaScript-Note/blob/master/notes/%E6%95%B0%E6%8D%AE%E7%B1%BB%E5%9E%8B.md#2数据类型的判断) 
- [null与undefined](https://github.com/AlfredYan/JavaScript-Note/blob/master/notes/%E6%95%B0%E6%8D%AE%E7%B1%BB%E5%9E%8B.md#3null与undefined) 
- [boolean](https://github.com/AlfredYan/JavaScript-Note/blob/master/notes/%E6%95%B0%E6%8D%AE%E7%B1%BB%E5%9E%8B.md#4boolean) 

## [对象](https://github.com/AlfredYan/JavaScript-Note/blob/master/notes/%E5%AF%B9%E8%B1%A1.md) 

- [对象概述](https://github.com/AlfredYan/JavaScript-Note/blob/master/notes/%E5%AF%B9%E8%B1%A1.md#1对象概述) 
- [创建对象的方法](https://github.com/AlfredYan/JavaScript-Note/blob/master/notes/%E5%AF%B9%E8%B1%A1.md#2创建对象的方法) 
- [对象的属性](https://github.com/AlfredYan/JavaScript-Note/blob/master/notes/%E5%AF%B9%E8%B1%A1.md#3对象的属性) 
- [属性（数据）描述符](https://github.com/AlfredYan/JavaScript-Note/blob/master/notes/%E5%AF%B9%E8%B1%A1.md#4属性数据描述符) 
- [访问描述符（getter & setter）](https://github.com/AlfredYan/JavaScript-Note/blob/master/notes/%E5%AF%B9%E8%B1%A1.md#5访问描述符getter--setter) 
- [对象/属性不变性](https://github.com/AlfredYan/JavaScript-Note/blob/master/notes/%E5%AF%B9%E8%B1%A1.md#6对象属性不变性) 
- [属性存在性](https://github.com/AlfredYan/JavaScript-Note/blob/master/notes/%E5%AF%B9%E8%B1%A1.md#7属性存在性) 
- [对象遍历](https://github.com/AlfredYan/JavaScript-Note/blob/master/notes/%E5%AF%B9%E8%B1%A1.md#8对象遍历) 
- [对象复制](https://github.com/AlfredYan/JavaScript-Note/blob/master/notes/%E5%AF%B9%E8%B1%A1.md#9对象复制) 
- [总结](https://github.com/AlfredYan/JavaScript-Note/blob/master/notes/%E5%AF%B9%E8%B1%A1.md#10总结) 


## [数值](https://github.com/AlfredYan/JavaScript-Note/blob/master/notes/%E6%95%B0%E5%80%BC.md#数值) 

- [数值类型概述](https://github.com/AlfredYan/JavaScript-Note/blob/master/notes/%E6%95%B0%E5%80%BC.md#1数值类型概述) 
- [数值的表示](https://github.com/AlfredYan/JavaScript-Note/blob/master/notes/%E6%95%B0%E5%80%BC.md#2数值的表示) 
- [数值的精度与范围](https://github.com/AlfredYan/JavaScript-Note/blob/master/notes/%E6%95%B0%E5%80%BC.md#3数值的精度与范围)
- [特殊的数值](https://github.com/AlfredYan/JavaScript-Note/blob/master/notes/%E6%95%B0%E5%80%BC.md#4特殊的数值) 
- [总结](https://github.com/AlfredYan/JavaScript-Note/blob/master/notes/%E6%95%B0%E5%80%BC.md#5总结) 

## [内置对象学习](https://github.com/AlfredYan/JavaScript-Note/tree/master/notes/%E5%86%85%E7%BD%AE%E5%AF%B9%E8%B1%A1) 

### [Object](https://github.com/AlfredYan/JavaScript-Note/blob/master/notes/%E5%86%85%E7%BD%AE%E5%AF%B9%E8%B1%A1/Object.md) 

- [new Object()](https://github.com/AlfredYan/JavaScript-Note/blob/master/notes/%E5%86%85%E7%BD%AE%E5%AF%B9%E8%B1%A1/Object.md#1new-object) 
- [Object()](https://github.com/AlfredYan/JavaScript-Note/blob/master/notes/%E5%86%85%E7%BD%AE%E5%AF%B9%E8%B1%A1/Object.md#2object) 
- [Object.defineProperty() && Object.defineProperties()](https://github.com/AlfredYan/JavaScript-Note/blob/master/notes/%E5%86%85%E7%BD%AE%E5%AF%B9%E8%B1%A1/Object.md#3objectdefineproperty--objectdefineproperties) 
- [Object.getOwnPropertyDescriptor()](https://github.com/AlfredYan/JavaScript-Note/blob/master/notes/%E5%86%85%E7%BD%AE%E5%AF%B9%E8%B1%A1/Object.md#4objectgetownpropertydescriptor) 
- [控制对象状态的方法：extension、seal、freeze](https://github.com/AlfredYan/JavaScript-Note/blob/master/notes/%E5%86%85%E7%BD%AE%E5%AF%B9%E8%B1%A1/Object.md#5控制对象状态的方法extensionsealfreeze) 
- [Object.getOwnPropertyNames() & Object.keys()](https://github.com/AlfredYan/JavaScript-Note/blob/master/notes/%E5%86%85%E7%BD%AE%E5%AF%B9%E8%B1%A1/Object.md#6objectgetownpropertynames--objectkeys) 
- [Object.getOwnPropertySymbols()](https://github.com/AlfredYan/JavaScript-Note/blob/master/notes/%E5%86%85%E7%BD%AE%E5%AF%B9%E8%B1%A1/Object.md#7objectgetownpropertysymbols) 
- [Object.entries() & Object.values()](https://github.com/AlfredYan/JavaScript-Note/blob/master/notes/%E5%86%85%E7%BD%AE%E5%AF%B9%E8%B1%A1/Object.md#8objectentries--objectvalues) 
- [Object.is()](https://github.com/AlfredYan/JavaScript-Note/blob/master/notes/%E5%86%85%E7%BD%AE%E5%AF%B9%E8%B1%A1/Object.md#9objectis) 
- [Obejct.assign()](https://github.com/AlfredYan/JavaScript-Note/blob/master/notes/%E5%86%85%E7%BD%AE%E5%AF%B9%E8%B1%A1/Object.md#10obejctassign) 
- [Object.create()](https://github.com/AlfredYan/JavaScript-Note/blob/master/notes/%E5%86%85%E7%BD%AE%E5%AF%B9%E8%B1%A1/Object.md#11objectcreate) 
- [Object.getPrototypeOf() & Object.setPrototypeOf()](https://github.com/AlfredYan/JavaScript-Note/blob/master/notes/%E5%86%85%E7%BD%AE%E5%AF%B9%E8%B1%A1/Object.md#12objectgetprototypeof--objectsetprototypeof) 
- [Object.prototype对象](https://github.com/AlfredYan/JavaScript-Note/blob/master/notes/%E5%86%85%E7%BD%AE%E5%AF%B9%E8%B1%A1/Object.md#13objectprototype对象) 


### [Number](https://github.com/AlfredYan/JavaScript-Note/blob/master/notes/%E5%86%85%E7%BD%AE%E5%AF%B9%E8%B1%A1/Number.md) 

- [简介](https://github.com/AlfredYan/JavaScript-Note/blob/master/notes/%E5%86%85%E7%BD%AE%E5%AF%B9%E8%B1%A1/Number.md#1简介) 
- [Number对象的属性](https://github.com/AlfredYan/JavaScript-Note/blob/master/notes/%E5%86%85%E7%BD%AE%E5%AF%B9%E8%B1%A1/Number.md#2number对象的属性) 
- [Number.isInteger() & Number.isSafeInteger()](https://github.com/AlfredYan/JavaScript-Note/blob/master/notes/%E5%86%85%E7%BD%AE%E5%AF%B9%E8%B1%A1/Number.md#3numberisinteger--numberissafeinteger) 
- [Number.isFinite() & Number.isNaN()](https://github.com/AlfredYan/JavaScript-Note/blob/master/notes/%E5%86%85%E7%BD%AE%E5%AF%B9%E8%B1%A1/Number.md#4numberisfinite--numberisnan) 
- [Number.parseInt() & Number.parseFloat()](https://github.com/AlfredYan/JavaScript-Note/blob/master/notes/%E5%86%85%E7%BD%AE%E5%AF%B9%E8%B1%A1/Number.md#5numberparseint--numberparsefloat) 
- [Number.prototype对象](https://github.com/AlfredYan/JavaScript-Note/blob/master/notes/%E5%86%85%E7%BD%AE%E5%AF%B9%E8%B1%A1/Number.md#6numberprototype对象) 