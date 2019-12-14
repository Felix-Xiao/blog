---
layout:     post
title:      javascript 中的对象
subtitle:   javascript 中的对象
date:       2019-06-19 12:03:16
tags  : 
  - 对象
  - js
  - 基础
author: Felix
location: Beijing  
---

# 什么是对象？

## 对象的本质

 1. 对象是具有唯一标识性的，即使完全相同的两个对象也不是同一个对象。
 2. 对象是具有状态的，同一对象可能处于不同状态之下。
 3. 对象是具有行为的，对象的状态可能因为他的行为发生改变。
																															 --《面向对象分析与设计》
																															
不同语言对于对象的描述：
 
  - 用类的方式描述对象   C++ Java
  - 用原型链的方式描述 JavaScript 

唯一标识  （内存地址）


语言     | 状态| 行为
-------- | -----| -----
C++  | 成员变量| 成员函数
java  | 属性| 方法
Javascript  | 属性| 属性

```javascript
// An highlighted block
var obj = {
	name："max",
	fn:function(){
	}
}
```
js 独特的特性： 对象具有高度动态性 这是因为js赋予使用者在运行时修改对象状态和行为的能力

提高抽象能力：
属性描述对象
	  - 数据属性  value writeable enumerable configurable
	  - 访问属性 get set

**前端属性描述对象** **元属性**：
> - **value** 属性是目标属性的值。
> - **writable** 属性是一个布尔值，决定了目标属性的值（value）是否可以被改变。
>- **enumerable**（可遍历性）返回一个布尔值，表示目标属性是否可遍历。
>- **configurable** (可配置性）返回一个布尔值，决定了是否可以修改属性描述对象。也 就是说，configurable为false时，value、writable、enumerable和configurable都不能被修改了。
>- **存取器**  除了直接定义以外，属性还可以用存取器（accessor）定义。其中，存值函数称为setter，使用属性描述对象的set属性；取值函数称为getter，使用属性描述对象的get属性。

数字的直接量

对象 属性

**装箱转换**

每一种基本类型Number String Boolean 在对象中都有对应的类（产生临时对象）
1 .toString()   => "1"

**拆箱操作**
    在js中，想要将对象转换成原始值，必然会调用toPrimitive()内部函数，那么它是如何工作的呢？

    该函数形式如下：
```javascript
toPrimitive(input,preferedType?)
```
   input是输入的值，preferedType是期望转换的类型，他可以是字符串，也可以是数字。

   如果转换的类型是number，会执行以下步骤：

   1. 如果input是原始值，直接返回这个值；

   2. 否则，如果input是对象，调用input.valueOf()，如果结果是原始值，返回结果；

   3. 否则，调用input.toString()。如果结果是原始值，返回结果；

   4. 否则，抛出错误。

   如果转换的类型是String，2和3会交换执行，即先执行toString()方法。

  你也可以省略preferedType，此时，日期会被认为是字符串，而其他的值会被当做Number。

   综上所述，会有以下计算结果：
```javascript
// An highlighted block
[] + [] //''
[] + {} //[Object, Object]
{} + [] //0
```

```
>[]+[]
>""
```

加号操作符会将preferedType看成Number，调用ES内部的toPrimitive(input，Number)方法，得到空字符串
```
>[]+{}
>"[object Object]"
```
 最终会调用双方的toString()方法，再做字符串加法
```
>{}+[]
>0
```
但是空对象加空数组就不一样了，加号运算符的定义是这样的：**如果其中一个是字符串，另一个也会被转换为字符串，否则两个运算数都被转换为数字**。而同时，javascript有这样的特性，**如果{}既可以被认为是代码块，又可以被认为是对象字面量，那么js会把他当做代码块来看待**。

这就很好解释了，{}被当做了代码块，只有+[]，根据加法的定义，被转换为0，就得到了结果。

**操作符中，==，排序运算符，加减乘除，在对非原始值进行操作时，都会调用内部的toPrimitive()方法。**