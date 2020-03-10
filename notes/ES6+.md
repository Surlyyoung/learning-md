---
title: ES6+
created: '2020-03-05T07:53:11.329Z'
modified: '2020-03-10T02:58:44.068Z'
---

# ES6+

### class的基本语法

**关键词**：`类` `类的原型prototype` `类的实例`


**class基本语法**
```

class Animal {
  // 类必须使用new调用
  // 类相当于实例的原型，所有在类中定义的方法，都会被实例继承

  // 实例属性除了定义在constructor()方法里面的this上面，也可以定义在类的最顶层
  nickName = '';

  // 类的默认方法，通过new命令生成对象实例时，自动调用该方法
  // 必须有constructor方法，如果没有显式定义，一个空的constructor方法会被默认添加
  // 默认返回对象this
  constructor(name) {
    // 实例的属性除非显式定义在其本身（即定义在this对象上），否则都是定义在原型上（即定义在class上）
    // 实例属性除了定义在constructor()方法里面的this上面，也可以定义在类的最顶层
    this.name = name;
  }

  // 存值函数和取值函数是设置在属性的 Descriptor 对象上的
  get prop() {
    return 'getter';
  }
  set prop(value) {
    console.log('setter: '+value);
  }

  // 类的所有方法都定义在类的prototype属性上面
  // 类内部定义的方法，它是不可枚举的
  bark() {}
}
```

**class继承--extend**

**extend**
```
class Point {}

class ColorPoint extends Point {
  constructor(x, y, color) {
    // 普通方法包括construtor里的super表示父类的构造函数，用来新建父类的this对象
    // 子类必须在constructor方法中调用super方法，否则 新建实例时 会报错
    super(x, y); // 调用父类的constructor(x, y)
    // 子类必须调用super()之后，才能使用this，因为此时this对象才构造完成
    this.color = color;
  }

  toString() {
    return this.color + ' ' + super.toString(); // 调用父类的toString()
  }
}

// 可以用来从子类上获取父类
// 可以使用这个方法判断，一个类是否继承了另一个类。
Object.getPrototypeOf(ColorPoint) === Point
// true
```
**super**
`super`这个关键字，既可以当作函数使用，也可以当作对象使用。
```
class A {
  constructor() {
    // new.target指向当前正在执行的函数
    console.log(new.target.name);
  }
}
class B extends A {
  constructor() {
    // super虽然代表了父类A的构造函数，但是返回的是子类B的实例，即super内部的this指的是B的实例，因此super()在这里相当于`A.prototype.constructor.call(this)`
    // 作为函数时，super()只能用在子类的构造函数之中，用在其他地方就会报错
    super();
  }
}
new A() // A
new B() // B
```
```
class A {
  constructor() {
    this.x = 1;
  }
  print() {
    console.log(this.x);
  }
  p() {
    return 2;
  }
}

class B extends A {
  constructor() {
    super();
    this.x = 2;

    super.x = 3; // 赋值时指向子类实例，相当于this.x=3
    console.log(super.x); // undefined 取值时指向父类的原型对象，相当于A.prototype.x

    // super作为对象时，在普通方法中，指向父类的原型对象；在静态方法中，指向父类。
    // 相当于 A.prototype.p()
    console.log(super.p()); // 2
  }
  m() {
    // 子类普通方法中通过super调用父类的方法时，方法内部的this指向当前的子类实例
    // 相当于 super.print.call(this)
    super.print();
  }
}

// 子类普通方法中通过super调用父类的方法时，方法内部的this指向当前的子类实例
let b = new B();
b.m() // 2 而不是1
```

如果`super`作为对象，用在静态方法之中，这时`super`将指向父类，而不是父类的原型对象。
```
class Parent {
  static myMethod(msg) {
    console.log('static', msg);
  }

  myMethod(msg) {
    console.log('instance', msg);
  }
}

class Child extends Parent {
  static myMethod(msg) {
    super.myMethod(msg);
  }

  myMethod(msg) {
    super.myMethod(msg);
  }
}

Child.myMethod(1); // static 1

var child = new Child();
child.myMethod(2); // instance 2

```
在子类的静态方法中通过`super`调用父类的方法时，方法内部的`this`指向当前的子类，而不是子类的实例。
```
class A {
  constructor() {
    this.x = 1;
  }
  static print() {
    console.log(this.x);
  }
}

class B extends A {
  constructor() {
    super();
    this.x = 2;
  }
  static m() {
    super.print();
  }
}

B.x = 3;
B.m() // 3
```
***由于对象总是继承其他对象的，所以可以在任意一个对象中，使用super关键字。***

**类的 prototype 属性和__proto__属性**
```
class A {
}

class B extends A {
}

// 继承之后，B本身是一个实例，B的prototype也是一个实例
B.__proto__ === A // true
B.prototype.__proto__ === A.prototype // true

// 以上代码是如此实现
class A {
}

class B {
}

// B 的实例继承 A 的实例
Object.setPrototypeOf(B.prototype, A.prototype);

// B 继承 A 的静态属性
Object.setPrototypeOf(B, A);

const b = new B();
```

**实例的 __proto__ 属性**

子类实例的__proto__属性的__proto__属性，指向父类实例的__proto__属性。也就是说，子类的原型的原型，是父类的原型
```
var p1 = new Point(2, 3);
var p2 = new ColorPoint(2, 3, 'red');

p2.__proto__ === p1.__proto__ // false
p2.__proto__.__proto__ === p1.__proto__ // true
```


---
