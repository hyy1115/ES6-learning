**本章涉及3个知识点，var、let、const，现在让我们了解3个关键字的特性和使用方法。**

### var
JavaScript中，我们通常说的作用域是函数作用域，使用var声明的变量，无论是在代码的哪个地方声明的，都会提升到当前作用域的最顶部，这种行为叫做**变量提升（Hoisting）**

也就是说，如果在函数内部声明的变量，都会被提升到该函数开头，而在全局声明的变量，就会提升到全局作用域的顶部。

```javascript
function test() {
    console.log('1: ', a) //undefined
    if (false) {
      var a = 1
    }
    console.log('3: ', a) //undefined
}

test()
```

实际执行时，上面的代码中的变量a会提升到函数顶部声明，即使if语句的条件是false，也一样不影响a变量提升。

```javascript
function test() {
    var a
    //a声明没有赋值
    console.log('1: ', a) //undefined
    if (false) {
      a = 1
    }
    //a声明没有赋值
    console.log('3: ', a) //undefined
}
```

在函数嵌套函数的场景下，变量只会提升到最近的一个函数顶部，而不会提升到外部函数。

```javascript
//b提升到函数a顶部，但不会提升到函数test。
    function test() {
        function a() {
          if (false) {
            var b = 2
          }
        }
        console.log('b: ', b)
    }
    
    test() //b is not defined
```

如果a没有声明，那么就会报错，**没有声明和声明后没有赋值是不一样的**，这点一定要区分开，有助于我们找bug。

```javascript
 //a没有声明的情况
    a is not defined
```

### let
let和const都能够声明块级作用域，用法和var是类似的，let的特点是不会变量提升，而是被锁在当前块中。

一个非常简单的例子：
```javascript

    function test() {
        if(true) {
          console.log(a)//TDZ，俗称临时死区，用来描述变量不提升的现象
          let a = 1
        }
    }
    test()  // a is not defined

    function test() {
        if(true) {
          let a = 1
        }
        console.log(a)
    }    
    test() // a is not defined
```

唯一正确的使用方法：**先声明，再访问。**

```javascript
function test() {
        if(true) {
          let a = 1
          console.log(a)
        }
    }
    test() // 1
```

### const
声明常量，一旦声明，不可更改，而且常量必须初始化赋值。

```javascript
const type = "ACTION"
```

我们试试重新声明type，看看会报什么错：
​    
```javascript
const type = "ACTION"
    type = 1
    console.log(type) //"type" is read-only
    
    const type = "ACTION"
    let type = 1
    console.log(type) //Duplicate declaration "type"
```

const虽然是常量，不允许修改默认赋值，但如果定义的是对象Object，那么可以修改对象内部的属性值包括新增删除键值对也是可以的。

```javascript
const type = {
      a: 1
    }
    type.a = 2 //没有直接修改type的值，而是修改type.a的属性值，这是允许的。
    console.log(type) // {a: 2}
    
    type.b = 3 //拓展Object也是没有问题的
    console.log(type) // {a: 2 , b: 3}
    
    delete type.b=3 //删除整个键值对也OK的
    console.log(type) // {a: 2}
    
    //如果重新定义数据结构~常量的内存地址值发生改变,这个是不可行的。
    type={}; //Assignment to constant variable.
    type=[]; //Assignment to constant variable.
```

### const和let的异同点

**相同点：**const和let都是在当前块内有效，执行到块外会被销毁，也不存在变量提升（TDZ），不能重复声明。

**不同点：**const不能再赋值，let声明的变量可以重复赋值。

### 临时死区(TDZ)
上面我们也提到了TDZ的场景，那么，有什么用呢？答案就是没什么用。

临时死区的意思是在当前作用域的块内，在声明变量前的区域叫做临时死区。
​    
```javascript
if (true) {
      //这块区域是TDZ
      let a = 1
    }
```

### 块级作用域的使用场景
除了上面提到的常用声明方式，我们还可以在循环中使用，最出名的一道面试题：循环中定时器闭包的考题

在for循环中使用var声明的循环变量，会跳出循环体污染当前的函数。

```javascript
for(var i = 0; i < 5; i++) {
      setTimeout(() => {
        console.log(i) //5, 5, 5, 5, 5
      }, 0)
    }
    console.log(i) //5 i跳出循环体污染外部函数
    
    //将var改成let之后
    for(let i = 0; i < 5; i++) {
      setTimeout(() => {
        console.log(i) // 0,1,2,3,4
      }, 0)
    }
    console.log(i)//i is not defined i无法污染外部函数
```

关于这个使用场景的具体分析可以查看我写的另外一篇文章：[JavaScript同步、异步、回调执行顺序之经典闭包setTimeout面试题分析][1]

### 在全局作用域声明
如果在全局作用域使用let或者const声明，当声明的变量本身就是全局属性，比如closed。只会覆盖该全局变量，而不会替换它。

```javascript
    window.closed = false
    let closed = true
    
    closed // true
    window.closed // false
```

### 最佳实践
在实际开发中，我们选择使用var、let还是const，取决于我们的变量是不是需要更新，通常我们希望变量保证不被恶意修改，而使用大量的const，在react中，props传递的对象是不可更改的，所以使用const声明，声明一个对象的时候，也推荐使用const，当你需要修改声明的变量值时，使用let，var能用的场景都可以使用let替代。


[1]: https://segmentfault.com/a/1190000008922457
[2]: https://segmentfault.com/a/1190000010199272

下一节：[2.字符串和正则表达式](https://github.com/hyy1115/ES6-learning/blob/master/doc/2%E3%80%81%E3%80%8A%E6%B7%B1%E5%85%A5%E7%90%86%E8%A7%A3ES6%E3%80%8B%E7%AC%94%E8%AE%B0%E2%80%94%E2%80%94%E5%AD%97%E7%AC%A6%E4%B8%B2%E5%92%8C%E6%AD%A3%E5%88%99%E8%A1%A8%E8%BE%BE%E5%BC%8F.md)
