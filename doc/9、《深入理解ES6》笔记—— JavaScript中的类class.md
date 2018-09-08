### ES5中的近类结构
ES5以及之前的版本，没有类的概念，但是聪明的JavaScript开发者，为了实现面向对象，创建了特殊的近类结构。

**ES5中创建类的方法：新建一个构造函数，定义一个方法并且赋值给构造函数的原型。**

```javascript
    'use strict';
    //新建构造函数，默认大写字母开头
    function Person(name) {
      this.name = name;
    }
    //定义一个方法并且赋值给构造函数的原型
    Person.prototype.sayName = function () {
      return this.name;
    };
    
    var p = new Person('eryue');
    console.log(p.sayName() // eryue
    );
```

### ES6 class类
ES6实现类非常简单，只需要类声明。推荐 [babel在线测试ES6][1] 测试下面的代码。

#### 类声明

如果你学过java，那么一定会非常熟悉这种声明类的方式。

```javascript
    class Person {
      //新建构造函数
      constructor(name) {
        this.name = name //私有属性
      }
      //定义一个方法并且赋值给构造函数的原型
      sayName() {
        return this.name
      }
    }
    let p = new Person('eryue')
    console.log(p.sayName()) // eryue
```

和ES5中使用构造函数不同的是，在ES6中，我们将原型的实现写在了类中，但本质上还是一样的，都是需要新建一个类名，然后实现构造函数，再实现原型方法。

**私有属性：**在class中实现私有属性，只需要在构造方法中定义this.xx = xx。

#### 类声明和函数声明的区别和特点

1、函数声明可以被提升，类声明不能提升。

2、类声明中的代码自动强行运行在严格模式下。

3、类中的所有方法都是不可枚举的，而自定义类型中，可以通过Object.defineProperty()手工指定不可枚举属性。

4、每个类都有一个[[construct]]的方法。

5、只能使用new来调用类的构造函数。

6、不能在类中修改类名。

#### 类表达式

类有2种表现形式：声明式和表达式。

```javascript
    //声明式
    class B {
      constructor() {}
    }

    //匿名表达式
    let A = class {
      constructor() {}
    }
    
    //命名表达式，B可以在外部使用，而B1只能在内部使用
    let B = class B1 {
      constructor() {}
    }
```

#### 类是一等公民

JavaScript函数是一等公民，类也设计成一等公民。

1、可以将类作为参数传入函数。

```javascript
    //新建一个类
    let A = class {
      sayName() {
        return 'eryue'
      }
    }
    //该函数返回一个类的实例
    function test(classA) {
      return new classA()
    }
    //给test函数传入A
    let t = test(A)
    console.log(t.sayName()) // eryue
```

2、通过立即调用类构造函数可以创建单例。

```javascript
    let a = new class {
      constructor(name) {
        this.name = name
      }
      sayName() {
        return this.name
      }
    }('eryue')
    console.log(a.sayName()) // eryue
```

#### 访问器属性

类支持在原型上定义访问器属性。
```javascript
    class A {
      constructor(state) {
        this.state = state
      }
      // 创建getter
      get myName() {
        return this.state.name
      }
      // 创建setter
      set myName(name) {
        this.state.name = name
      }
    }
    // 获取指定对象的自身属性描述符。自身属性描述符是指直接在对象上定义（而非从对象的原型继承）的描述符。
    let desriptor = Object.getOwnPropertyDescriptor(A.prototype, "myName")
    console.log("get" in desriptor) // true
    console.log(desriptor.enumerable) // false 不可枚举
```

#### 可计算成员名称

可计算成员是指使用方括号包裹一个表达式，如下面定义了一个变量m，然后使用[m]设置为类A的原型方法。
```javascript
    let m = "sayName"
    class A {
      constructor(name) {
        this.name = name
      }
      [m]() {
        return this.name
      }
    }
    let a = new A("eryue")
    console.log(a.sayName()) // eryue
```

#### 生成器方法

回顾一下上一章讲的生成器，生成器是一个返回迭代器的函数。在类中，我们也可以使用生成器方法。

```javascript
    class A {
      *printId() {
        yield 1;
        yield 2;
        yield 3;
      }
    }
    let a = new A()
    let x = a.printId()
    
    x.next() // {done: false, value: 1}
    x.next() // {done: false, value: 2}
    x.next() // {done: false, value: 3}
```

这个写法很有趣，我们新增一个原型方法稍微改动一下。

```javascript
    class A {
      *printId() {
        yield 1;
        yield 2;
        yield 3;
      }
      render() {
      //从render方法访问printId，很熟悉吧，这就是react中经常用到的写法。
        return this.printId()
      }
    }
    let a = new A()
    console.log(a.render().next()) // {done: false, value: 1}
```

#### 静态成员

静态成员是指在方法名或属性名前面加上static关键字，和普通方法不一样的是，static修饰的方法不能在实例中访问，只能在类中直接访问。
```javascript
    class A {
      constructor(name) {
        this.name = name
      }
      static create(name) {
        return new A(name)
      }
    }
    let a = A.create("eryue")
    console.log(a.name) // eryue
    
    let t = new A()
    console.log(t.create("eryue")) // t.create is not a function
```

#### 继承与派生类

我们在写react的时候，自定义的组件会继承React.Component。

```javascript
    class A extends Component {
       constructor(props){
           super(props)
       }
    }
```

**A叫做派生类**，在派生类中，如果使用了构造方法，就必须使用super()。

```javascript
    class Component {
      constructor([a, b] = props) {
        this.a = a
        this.b = b
      }
      add() {
        return this.a + this.b
      }
    }
    
    class T extends Component {
      constructor(props) {
        super(props)
      }
    }
    
    let t = new T([2, 3])
    console.log(t.add()) // 5
```

**关于super使用的几点要求：**

1、只可以在派生类中使用super。派生类是指继承自其它类的新类。

2、在构造函数中访问this之前要调用super()，负责初始化this。

```javascript
    class T extends Component {
      constructor(props) {
        this.name = 1 // 错误，必须先写super()
        super(props)
      }
    }
```

3、如果不想调用super，可以让类的构造函数返回一个对象。

#### 类方法遮蔽

我们可以在继承的类中重写父类的方法。
```javascript
    class Component {
      constructor([a, b] = props) {
        this.a = a
        this.b = b
      }
      //父类的add方法，求和
      add() {
        return this.a + this.b
      }
    }
    
    class T extends Component {
      constructor(props) {
        super(props)
      }
      //重写add方法，求积
      add() {
        return this.a * this.b
      }
    }
    let t = new T([2, 3])
    console.log(t.add()) // 6
```

#### 静态成员继承

**父类中的静态成员，也可以继承到派生类中。**静态成员继承只能通过派生类访问，不能通过派生类的实例访问。

```javascript
    class Component {
      constructor([a, b] = props) {
        this.a = a
        this.b = b
      }
      static printSum([a, b] = props) {
        return a + b
      }
    }
    class T extends Component {
      constructor(props) {
        super(props)
      }
    }
    console.log(T.printSum([2, 3])) // 5
```

#### 派生自表达式的类

很好理解，就是指父类可以是一个表达式。

#### 内建对象的继承

有些牛逼的人觉得使用内建的Array不够爽，就希望ECMA提供一种继承内建对象的方法，然后那帮大神们就把这个功能添加到class中了。
```javascript
    class MyArray extends Array { }
    let colors = new MyArray()
    colors[0] = "1"
    console.log(colors) // [1]
```

#### Symbol.species

该用法我还没有接触过，目前只知道在内建对象中使用了该方法，如果在类中调用this.constructor，使用Symbol.species可以让派生类重写返回类型。

#### 在构造函数中使用new.target

new.target通常表示当前的构造函数名。通常我们使用new.target来阻止直接实例化基类，下面是这个例子的实现。

```javascript
    class A {
      constructor() {
      //如果当前的new.target为A类，就抛出异常
        if (new.target === A) {
          throw new Error("error haha")
        }
      }
    }
    let a = new A()
    console.log(a) // error haha
```

### 总结

本章只有一个知识点，那就是class的使用，最开始的声明class，到后面的继承派生类，都是非常常用的写法，还有静态成员的使用。

如果上面的那些例子你练习的不够爽，或许你该找个react基础demo简单的使用class来练练手了。

[=> 返回文章目录][2]


[1]: https://babeljs.io/repl/#?babili=false&amp;evaluate=true&amp;lineWrap=true&amp;presets=env,es2015,es2015-loose,es2016,es2017,stage-0,stage-1,stage-2,stage-3&amp;targets=&amp;browsers=%3E%202%25,%20ie%209,&amp;builtIns=false&amp;debug=false&amp;code_lz=MYGwhgzhAECC0G8BQ1rAPYDsIBcBOArsDungBS5g4CmAlIiqtDgBYCWEAdJTdALzQe1RgF9GAc2o5oAWwCeAOTAzqZesibQ8UgnkzN2XIZ0zLhqMaghTZis2VMr1jVKw7ccVaibP9oj82gxMRAbABNqCDw2AAcSPD8AeQAjACtqYk5JHESAd0wABTx0GOo8HDkAEUjgaLjSMlhOGOKSCtKAGmgAInklFW7aJAxsdFDOEHRxMgio2PjOakwCFTwwZNDaIA
[2]: https://segmentfault.com/a/1190000010199272

上一节：[8.迭代器（Iterator）和生成器（Generator）](https://github.com/hyy1115/ES6-learning/blob/master/doc/8%E3%80%81%E3%80%8A%E6%B7%B1%E5%85%A5%E7%90%86%E8%A7%A3ES6%E3%80%8B%E7%AC%94%E8%AE%B0%E2%80%94%E2%80%94%E8%BF%AD%E4%BB%A3%E5%99%A8%EF%BC%88Iterator%EF%BC%89%E5%92%8C%E7%94%9F%E6%88%90%E5%99%A8%EF%BC%88Generator%EF%BC%89.md)

下一节：[10.改进数组的功能](https://github.com/hyy1115/ES6-learning/blob/master/doc/10%E3%80%81%E3%80%8A%E6%B7%B1%E5%85%A5%E7%90%86%E8%A7%A3ES6%E3%80%8B%E7%AC%94%E8%AE%B0%E2%80%94%E2%80%94%20%E6%94%B9%E8%BF%9B%E6%95%B0%E7%BB%84%E7%9A%84%E5%8A%9F%E8%83%BD.md)