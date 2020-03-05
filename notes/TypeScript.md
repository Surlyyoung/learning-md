---
title: TypeScript
created: '2020-02-17T14:22:33.508Z'
modified: '2020-03-04T10:16:52.831Z'
---

# TypeScript

参考文章：[阮一峰Typescript入门](ts.xcatliu.com "阮一峰Typescript入门")

**本文所有知识点都在围绕类型（type），无论数组、元组、枚举、接口、类都是type**

### 基础
1. typescript 如果没有明确的指定类型，那么 TypeScript 会依照 **类型推论（Type Inference）** 的规则推断出一个类型；如果定义的时候没有赋值，不管之后有没有赋值，都会被推断成 any 类型而完全不被类型检查；

2. **联合类型**（Union Types）表示取值可以为多种类型中的一种。用|分隔类型；

3. 使用 **接口（Interfaces）** 来定义 **对象的类型**；接口中有可选属性、任意属性、刻度属性
  > 接口：在面向对象语言中，接口（Interfaces）是一个很重要的概念，它是对行为的抽象，而具体如何行动需要由类（classes）去实现（implement）；

  > TypeScript 中的接口是一个非常灵活的概念，除了可用于对类的一部分行为进行抽象以外，也常用于对「对象的形状（Shape）」进行描述
  >> 动物都会叫（有叫的方法），不同类的动物叫法不一样（每一类动物叫出的声音不同，这是具体的实现）

4. **数组类型** 
+ 类型 + 方括号 `number[] string[]`;
+ 数组泛型（Array Generic）`Array<number>`;
+ 接口也可以用来描述数组(通常用于类数组，类数组有内置的接口定义IArguments等) `{[index: number]: number}`;
+ any应用 `any[]`

5. **函数类型** 
+ 函数定义 `function (x: number, y: number): number`，可以有可选参数，必须在必选之后；可以有默认值，TypeScript 会将添加了默认值的参数识别为可选参数
+ 使用接口的方式来定义一个函数需要符合的形状

6. **类型断言（Type Assertion）** 可以用来手动指定一个值的类型
+ `<类型>值`断言的变量前加上 <Type> 或 `值 as 类型`
+ 一个联合类型的变量指定为一个更加具体的类型
+ 类型断言不是类型转换，断言成一个联合类型中不存在的类型是不允许的

7. **声明文件**
+ 通常我们会把声明语句放到一个单独的文件（jQuery.d.ts）中，这就是声明文件
+ 声明文件必需以 .d.ts 为后缀
+ 书写声明文件--（[查看](https://ts.xcatliu.com/basics/declaration-files#shu-xie-sheng-ming-wen-jian)） 

8. **内置对象**
+ 内置对象是指根据标准在**全局作用域（Global）**上存在的对象
+ ECMAScript 标准提供的内置对象`Boolean` `Error` `Date` `RegExp`等，可以在 TypeScript 中将变量定义为这些类型
+ DOM 和 BOM 提供的内置对象`Document、HTMLElement、Event、NodeList`,TypeScript 中会经常用到这些类型

---

### 进阶
1. **类型别名**
+ type==类型，使用 `type` 创建类型别名，给一个类型起个新名字，`type Name = string;`
+ 类型别名常用于**联合类型**

2. **字符串字面量类型**
+ 字符串字面量类型用来约束取值只能是某几个字符串中的一个
+ **类型别名**与**字符串字面量类型**都是使用 `type` 进行定义，`type EventNames = 'click' | 'scroll' | 'mousemove';`

3. **元组**
+ 数组合并了相同类型的对象，而**元组（Tuple）合并了不同类型的对象**
+ 添加**越界**的元素时，它的类型会被限制为元组中每个类型的**联合类型**
```
let tom: [string, number] = ['Tom', 25];
tom.push('male');
tom.push(true);

// Argument of type 'true' is not assignable to parameter of type 'string | number'.
```

4. **枚举（Enum）**
+ 枚举（Enum）类型用于取值被限定在一定范围内的场景，如一周只能有七天
+ 枚举使用 `enum` 关键字来定义
+ 枚举成员会**被赋值为从 0 开始递增**的数字，同时**也会对枚举值到枚举名进行反向映射**
+ 可以给枚举项**手动赋值**，未手动赋值的枚举项会接着上一个枚举项递增
+ 未手动赋值的枚举项与手动赋值的重复了，TypeScript 会覆盖
+ 手动赋值的枚举项可以不是数字，此时需要使用类型断言来让 tsc 无视类型检查
+ 手动赋值的枚举项也可以为小数或负数，此时后续未手动赋值的项的递增步长仍为 1
+ 枚举项有两种类型：**常数项（constant member）**和**计算所得项（computed member）**
+ 如果紧接在计算所得项后面的是未手动赋值的项，那么它就会因为无法获得初始值而报错
+ **常数枚举**是使用` const enum `定义的枚举类型
+ 常数枚举与普通枚举的区别是，它会在编译阶段被删除，并且不能包含计算成员
+ **外部枚举（Ambient Enums）**是使用` declare enum `定义的枚举类型
+ 外部枚举与声明语句一样，常出现在**声明文件**中，同时使用 `declare` 和 `const` 也是可以的
```
enum Days {Sun, Mon, Tue, Wed, Thu, Fri, Sat};

console.log(Days["Sun"] === 0); // true
console.log(Days["Mon"] === 1); // true
console.log(Days["Tue"] === 2); // true
console.log(Days["Sat"] === 6); // true

console.log(Days[0] === "Sun"); // true
console.log(Days[1] === "Mon"); // true
console.log(Days[2] === "Tue"); // true
console.log(Days[6] === "Sat"); // true
```
上面的例子会被编译为：
```
var Days;
(function (Days) {
    Days[Days["Sun"] = 0] = "Sun";
    Days[Days["Mon"] = 1] = "Mon";
    Days[Days["Tue"] = 2] = "Tue";
    Days[Days["Wed"] = 3] = "Wed";
    Days[Days["Thu"] = 4] = "Thu";
    Days[Days["Fri"] = 5] = "Fri";
    Days[Days["Sat"] = 6] = "Sat";
})(Days || (Days = {}));
```
```
计算所得项：
enum Color {Red = "red".length, Green, Blue};

// index.ts(1,33): error TS1061: Enum member must have initializer.
// index.ts(1,40): error TS1061: Enum member must have initializer.
```
```
常数枚举：
const enum Directions {
    Up,
    Down,
    Left,
    Right
}

let directions = [Directions.Up, Directions.Down, Directions.Left, Directions.Right];
```
```
外部枚举（外部常数枚举）：
declare const enum Directions {
    Up,
    Down,
    Left,
    Right
}

let directions = [Directions.Up, Directions.Down, Directions.Left, Directions.Right];
```

5. **类（class）**
+ es6以前，通过**构造函数**实现类的概念，通过**原型链**实现继承，es6后有类的概念-`class`
+ 类(Class)：定义了一件事物的抽象特点，包含它的属性和方法；
+ 对象（Object）：类的实例，通过 new 生成；
+ 面向对象（OOP）的三大特性：封装（Encapsulation）、继承（Inheritance）、多态（Polymorphism）；
> 封装（Encapsulation）：将对数据的操作细节隐藏起来，只暴露对外的接口。外界调用端不需要（也不可能）知道细节，就能通过**对外提供的接口**来**访问该对象**，同时也保证了外界**无法任意更改对象内部**的数据

> 继承（Inheritance）：子类继承父类，子类除了拥有父类的所有特性外，还有一些更具体的特性

> 多态（Polymorphism）：由继承而产生了相关的不同的类，对同一个方法可以有不同的响应。比如 `Cat` 和 `Dog` 都继承自 `Animal`，但是分别实现了自己的 `eat` 方法。此时针对某一个实例，我们无需了解它是 `Cat` 还是 `Dog`，就可以直接调用 `eat` 方法，程序会自动判断出来应该如何执行 `eat`

+ 存取器（getter & setter）：用以改变属性的读取和赋值行为
+ 修饰符（Modifiers）：修饰符是一些关键字，用于限定成员或类型的性质。比如 public 表示公有属性或方法

+ 抽象类（Abstract Class）：抽象类是**供其他类继承**的**基类**，抽象类**不允许被实例化**。抽象类中的**抽象方法必须在子类中被实现**
+ 接口（Interfaces）：**不同类之间公有的属性或方法，可以抽象成一个接口**。接口可以**被类实现（implements）**。一个类**只能继承自另一个类**，但是**可以实现多个接口**

#### ES6 中类的用法
```
// class定义类，(对象的模板，提供复制)
class Animal {
    // constructor定义构造函数，默认返回实例对象（即this）
    // constructor方法是类的默认方法，如果没有显式定义，一个空的constructor方法会被默认添加
    constructor(name) {
        // 实例的属性除非显式定义在其本身（即定义在this对象上），否则都是定义在原型上（即定义在class上）
        this.name = name;
    }
    // 事实上，类的所有方法都定义在类的prototype属性上面
    // 实例的属性除非显式定义在其本身（即定义在this对象上），否则都是定义在原型上（即定义在class上）
    sayHi() {
        // 类中方法如果含有this，默认指向类的实例
        return `My name is ${this.name}`;
    }
}

// new生成类的实例对象，生成实例的时候会调用类的构造函数constructor(name)
let a = new Animal('Jack');
console.log(a.sayHi()); // My name is Jack

typeof Animal // "function"
Animal === Animal.prototype.constructor // true

// es5实现类的写法，上面es6 class的写法相当于语法糖
function Animal(name){
  this.name = name;
}
Animal.prototype.sayHi = function() {
  return `My name is ${this.name}`;
}
```

```
// extends 关键字实现继承
class Cat extends Animal {
    constructor(name) {
        // 子类中使用 super 关键字来调用父类的构造函数和方法
        super(name); // 调用父类的 constructor(name)
        console.log(this.name);
    }
    sayHi() {
        // 子类中使用 super 关键字来调用父类的构造函数和方法
        return 'Meow, ' + super.sayHi(); // 调用父类的 sayHi()
    }
}

let c = new Cat('Tom'); // Tom
console.log(c.sayHi()); // Meow, My name is Tom
```

```
// 使用 static 修饰符修饰的方法称为静态方法，不需要实例化，而是直接通过类来调用
class Animal {
    static isAnimal(a) {
        return a instanceof Animal;
    }
}

let a = new Animal('Jack');
Animal.isAnimal(a); // true
a.isAnimal(a); // TypeError: a.isAnimal is not a function
```

#### typescript中类的使用
[public,private,protected三者针对类、类的外部、子类描述](https://ts.xcatliu.com/advanced/class#typescript-zhong-lei-de-yong-fa) 
+ `public` 修饰的属性或方法是公有的，可以在任何地方被访问到，默认所有的属性和方法都是 public 的
+ `private` 修饰的属性或方法是私有的，不能在声明它的类的外部访问
> 当构造函数修饰为 private 时，该类不允许被继承或者实例化
+ `protected` 修饰的属性或方法是受保护的，它和 private 类似，区别是它在子类中也是允许被访问的
> 当构造函数修饰为 protected 时，该类只允许被继承


---

#### 类实现接口
一个类只能继承自另一个类，有时候**不同类**之间可以有一些**共有的特性**，这时候就可以把特性提取成接口（interfaces），用 `implements` 关键字来实现
```
interface Alarm {
    alert(): void;
}

class Door {
}

class SecurityDoor extends Door implements Alarm {
    alert() {
        console.log('SecurityDoor alert');
    }
}

class Car implements Alarm {
    alert() {
        console.log('Car alert');
    }
}
```
+ 一个类可以实现多个接口
```
interface Alarm {
    alert(): void;
}

interface Light {
    lightOn(): void;
    lightOff(): void;
}

class Car implements Alarm, Light {
    alert() {
        console.log('Car alert');
    }
    lightOn() {
        console.log('Car light on');
    }
    lightOff() {
        console.log('Car light off');
    }
}
```
+ 接口继承接口
```
interface Alarm {
    alert(): void;
}

interface LightableAlarm extends Alarm {
    lightOn(): void;
    lightOff(): void;
}
```
+ 接口继承类（实例的类型）
**当我们在声明 class Point 时，除了会创建一个名为 Point 的类之外，同时也创建了一个名为 Point 的类型（实例的类型）**
```
class Point {
    x: number;
    y: number;
    constructor(x: number, y: number) {
        this.x = x;
        this.y = y;
    }
}

interface Point3d extends Point {
    z: number;
}

let point3d: Point3d = {x: 1, y: 2, z: 3};
```
接口继承类的时候，也**只会继承它的实例属性和实例方法**，除了构造函数是不包含的，静态属性或静态方法也是不包含的（实例的类型当然不应该包括构造函数、静态属性或静态方法）
```
class Point {
    x: number;
    y: number;
    constructor(x: number, y: number) {
        this.x = x;
        this.y = y;
    }
}

interface PointInstanceType {
    x: number;
    y: number;
}

// 等价于 interface Point3d extends PointInstanceType
interface Point3d extends Point {
    z: number;
}

let point3d: Point3d = {x: 1, y: 2, z: 3};
```
```
class Point {
    /** 静态属性，坐标系原点 */
    static origin = new Point(0, 0);
    /** 静态方法，计算与原点距离 */
    static distanceToOrigin(p: Point) {
        return 
    }
    /** 实例属性，x 轴的值 */
    x: number;
    /** 实例属性，y 轴的值 */
    y: number;
    /** 构造函数 */
    constructor(x: number, y: number) {
        this.x = x;
        this.y = y;
    }
    /** 实例方法，打印此点 */
    printPoint() {
        console.log(this.x, this.y);
    }
}

interface PointInstanceType {
    x: number;
    y: number;
    printPoint(): void;
}

// 两者等价
let p1: Point;
let p2: PointInstanceType;
```

---

6. **泛型**
泛型（Generics）是指在**定义函数、接口或类的时候，不预先指定具体的类型，而在使用的时候再指定类型的一种特性**
```
// 函数名后添加了 <T>，其中 T 用来指代任意输入的类型，在后面的输入 value: T 和输出 Array<T> 中即可使用
function createArray<T>(length: number, value: T): Array<T> {
    let result: T[] = [];
    for (let i = 0; i < length; i++) {
        result[i] = value;
    }
    return result;
}

// 调用的时候，可以指定它具体的类型为 string。当然，也可以不手动指定，而让类型推论自动推算出来
createArray<string>(3, 'x'); // ['x', 'x', 'x']
```
**定义泛型的时候，可以一次定义多个类型参数**
```
function swap<T, U>(tuple: [T, U]): [U, T] {
    return [tuple[1], tuple[0]];
}

swap([7, 'seven']); // ['seven', 7]
```
**泛型约束**
```
interface Lengthwise {
    length: number;
}

// 使用了 extends 约束了泛型 T 必须符合接口 Lengthwise 的形状
function loggingIdentity<T extends Lengthwise>(arg: T): T {
    console.log(arg.length);
    return arg;
}
```
```
// 多个类型参数之间也可以互相约束
function copyFields<T extends U, U>(target: T, source: U): T {
    for (let id in source) {
        target[id] = (<T>source)[id];
    }
    return target;
}

let x = { a: 1, b: 2, c: 3, d: 4 };

copyFields(x, { b: 10, d: 20 });
```
```
// 泛型接口
interface CreateArrayFunc {
    <T>(length: number, value: T): Array<T>;
}

let createArray: CreateArrayFunc;
createArray = function<T>(length: number, value: T): Array<T> {
    let result: T[] = [];
    for (let i = 0; i < length; i++) {
        result[i] = value;
    }
    return result;
}

createArray(3, 'x'); // ['x', 'x', 'x']
```
```
// 泛型类
class GenericNumber<T> {
    zeroValue: T;
    add: (x: T, y: T) => T;
}

let myGenericNumber = new GenericNumber<number>();
myGenericNumber.zeroValue = 0;
myGenericNumber.add = function(x, y) { return x + y; };
```

```
// 泛型参数的默认类型
function createArray<T = string>(length: number, value: T): Array<T> {
    let result: T[] = [];
    for (let i = 0; i < length; i++) {
        result[i] = value;
    }
    return result;
}
```

7. **声明合并**
[函数合并，接口合并(属性的类型要一致)，类合并](https://ts.xcatliu.com/advanced/declaration-merging)

---

