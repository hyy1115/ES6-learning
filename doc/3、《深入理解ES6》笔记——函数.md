作为前端切图仔，越发觉得自己离不开函数了。

说到JavaScript函数，脑子里都是匿名函数、普通函数、闭包函数、构造函数......然后还能说出一大堆函数的概念。如果你达到这个水平，那么函数对你来说没有难度，是一个前端老鸟了。

当我闭上眼睛，不看键盘，手指在键盘上敲击出一个排序函数的时候，我在想，还是回顾一下函数的基本知识才有意思。

### 函数的默认参数
在ES5中，我们给函数传参数，然后在函数体内设置默认值，如下面这种方式。

```javascript
    function a(num, callback) {
      num = num || 6
      callback = callback || function (data) {console.log('ES5: ', data)}
      callback(num * num)
    }
    a() //ES5: 36，不传参输出默认值
    
    //你还可以这样使用callback
    a(10, function(data) {
      console.log(data * 10) // 1000， 传参输出新数值
    })
```

**而在ES6中，我们使用新的默认值写法。**

```javascript
    function a(num = 6, callback = function (data) {console.log('ES6: ', data)}) {
      callback(num * num)
    }
    
    a() //ES6: 36， 不传参输出默认值
    
    a(10, function(data) {
      console.log(data * 10) // 1000，传参输出新数值
    })
```

**使用ES6的默认值写法可以让函数体内部的代码更加简洁优雅**

**默认值对arguments对象的影响**

我们先要了解arguments对象是什么？准确一点来说它是一个类数组对象，它存在函数内部，它将当前函数的所有参数组成了一个类数组对象。

```javascript
    function a(num, b){
      console.log(arguments) // {"0": 6, "1": 10}
      console.log(arguments.length) // 2
    }
    
    a(6, 10) 
```

上面的输出结果看起来很正常，那么，如果我们加上参数默认值会怎样呢？

```javascript
    function a(num = 1, b = 1){
      console.log(arguments)
    }
    a() // {} 默认值不能被arguments识别。
    a(6, 10) // {"0":6,"1":10}
```

下面我们看一下修改参数默认值对arguments的影响。

1、在ES5的非严格模式下，一开始输入的参数是1，那么可以获取到arguments[0]（表示第一个参数）全等于num，修改num = 2之后，arguments[0]也能更新到2。

```javascript
    function a(num){
      console.log(num === arguments[0]) //true
      num = 2 //修改参数默认值
      console.log(num === arguments[0]) //true
    }
    a(1)
```

2、在ES5的严格模式下，arguments就不能在函数内修改默认值后跟随着跟新了。

```javascript
    "use strict"; //严格模式   
    function a(num) {
      console.log(num === arguments[0]); // true
      num = 2;
      console.log(num === arguments[0]); // false
    }
    a(1);
```
**在ES6环境下，默认值对arguments的影响和ES5严格模式是同样的标准。**

**默认参数表达式**

参数不仅可以设置默认值为字符串，数字，数组或者对象，还可以是一个函数。

```javascript
    function add() {
      return 10
    }
    function a(num = add()){
      console.log(num)
    }
    a() // 10
```
**默认参数的临时死区**

第一章我们提到了let和const什么变量的临时死区（TDZ），默认参数既然是参数，那么也同样有临时死区，函数的作用域是独立的，a函数不能共享b函数的作用域参数。

```javascript
    //这是个默认参数临时死区的例子，当初始化a时，b还没有声明，所以第一个参数对b来说就是临时死区。
    function add(a = b, b){
      console.log(a + b)
    }
    add(undefined, 2) // b is not define
```

### 无命名参数

上面说的参数都是命名参数，而无命名参数也是函数传参时经常用到的。当传入的参数是一个对象，不是一个具体的参数名，则是无命名参数。

```javascript
    function add(object){
      console.log(object.a + object.b)
    }
    let obj = {
      a: 1,
      b: 2
    }
    add(obj) // 3
```

**不定参数的使用：**使用...（展开运算符）的参数就是不定参数，它表示一个数组。

```javascript
    function add(...arr){
      console.log(a + b)
    }
    let a = 1,b = 2
    add(a, b) // 3
```

**不定参数的使用限制：**必须放在所有参数的末尾，不能用于对象字面量setter中。

```javascript
    //错误的写法1
    function add(...arr, c){
      console.log(a + b)
    }
    let a = 1,b = 2,c = 3
    add(a, b, c)
    
    //错误的写法2
    let obj = {
      set add(...arr) {
      
      }
    }
```

**ES6中的构造函数Function新增了支持默认参数和不定参数。**

### 展开运算符（...）

展开运算符的作用是解构数组，然后将每个数组元素作为函数参数。

有了展开运算符，我们操作数组的时候，就可以不再使用apply来指定上下文环境了。

```javascript
    //ES5的写法
    let arr = [10, 20, 50, 40, 30]
    let a = Math.max.apply(null, arr)
    console.log(a) // 50
    
    //ES6的写法
    let arr = [10, 20, 50, 40, 30]
    let a = Math.max(...arr)
    console.log(a) // 50
```

### 块级函数

**严格模式下：**在ES6中，你可以在块级作用域内声明函数，该函数的作用域只限于当前块，不能在块的外部访问。
```javascript
    "use strict";
    if(true) {
      const a = function(){
      
      }
    } 
```
**非严格模式：**即使在ES6中，非严格模式下的块级函数，他的作用域也会被提升到父级函数的顶部。所以大家写代码尽量使用严格模式，避免这些奇葩情况。

### 箭头函数（=>）

如果看到你这里，你发现你还没有在项目中使用过箭头函数，没关系，你并不low，而是学习不够努力。

```javascript
    const arr = [5, 10]
    const s = arr.reduce((sum, item) => sum + item)
    console.log(s) // 15
```

**箭头函数和普通函数的区别是：**

1、箭头函数没有this，函数内部的this来自于父级最近的非箭头函数，并且不能改变this的指向。

2、箭头函数没有super

3、箭头函数没有arguments

4、箭头函数没有new.target绑定。

5、不能使用new

6、没有原型

7、不支持重复的命名参数。

**箭头函数的简单理解**

1、箭头函数的左边表示输入的参数，右边表示输出的结果。

```javascript
    const s = a => a
    console.log(s(2)) // 2
```

2、箭头函数中，最重要的this报错将不再成为你每天都担心的bug。

3、箭头函数还可以输出对象，在react的action中就推荐这种写法。

```javascript
    const action = (type, a) => ({
      type: "TYPE",
      a
    })
```

4、支持立即执行函数表达式写法

```javascript
    const test = ((id) => {
      return {
        getId() {
          console.log(id)
        }
      }
    })(18)
    test.getId() // 18
```

5、箭头函数给数组排序

```javascript
    const arr = [10, 50, 30, 40, 20]
    const s = arr.sort((a, b) => a - b)
    console.log(s) // [10,20,30,40,50]
```

### 尾调用优化

尾调用是什么鬼？

尾调用是指在函数return的时候调用一个新的函数，由于尾调用的实现需要存储到内存中，在一个循环体中，如果存在函数的尾调用，你的内存可能爆满或溢出。

ES6中，引擎会帮你做好尾调用的优化工作，你不需要自己优化，但需要满足下面3个要求：

1、函数不是闭包

2、尾调用是函数最后一条语句

3、尾调用结果作为函数返回

**一个满足以上要求的函数如下所示：**

```javascript
    "use strict";   
    function a() {
      return b();
    }
```

**下面的都是不满足的写法：**

```javascript
    //没有return不优化
    "use strict";
    function a() {
      b();
    }
    
    //不是直接返回函数不优化
    "use strict";
    function a() {
      return 1 + b();
    }
    
    //尾调用是函数不是最后一条语句不优化
    "use strict";
    function a() {
      const s = b();
      return s
    }
    
    //闭包不优化
    "use strict";
    function a() {
      const num = 1
      function b() {
        return num
      }
      return b
    }
```

**尾调用实际用途——递归函数优化**

在ES5时代，我们不推荐使用递归，因为递归会影响性能。

但是有了尾调用优化之后，递归函数的性能有了提升。

```javascript
    //新型尾优化写法
    "use strict";  
    function a(n, p = 1) {
      if(n <= 1) {
        return 1 * p
      }
      let s = n * p
      return a(n - 1, s)
    }
    //求 1 x 2 x 3的阶乘
    let sum = a(3)
    console.log(sum) // 6
```

### 总结

函数这一章涉及到的知识点比较多，默认参数，命名参数，不定参数，展开运算符，箭头函数，尾调用优化。

第一次学习这些知识的人可以关注箭头函数和展开运算符的使用，这是最重要也最常用的知识，如果你已经在项目中使用过这些知识，那么作为巩固也是有帮助的，俗话说温故知新。



[1]: https://segmentfault.com/a/1190000010199272

上一节：[2.字符串和正则表达式](https://github.com/hyy1115/ES6-learning/blob/master/doc/2%E3%80%81%E3%80%8A%E6%B7%B1%E5%85%A5%E7%90%86%E8%A7%A3ES6%E3%80%8B%E7%AC%94%E8%AE%B0%E2%80%94%E2%80%94%E5%AD%97%E7%AC%A6%E4%B8%B2%E5%92%8C%E6%AD%A3%E5%88%99%E8%A1%A8%E8%BE%BE%E5%BC%8F.md)

下一节：[4.扩展对象的功能性](https://github.com/hyy1115/ES6-learning/blob/master/doc/4%E3%80%81%E3%80%8A%E6%B7%B1%E5%85%A5%E7%90%86%E8%A7%A3ES6%E3%80%8B%E7%AC%94%E8%AE%B0%E2%80%94%E2%80%94%E6%89%A9%E5%B1%95%E5%AF%B9%E8%B1%A1%E7%9A%84%E5%8A%9F%E8%83%BD%E6%80%A7.md)