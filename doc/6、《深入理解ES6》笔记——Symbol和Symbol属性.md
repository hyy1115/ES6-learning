还记得对象Object吗？

```javascript
    let obj = {
      a: 1
    }
```
对象的格式：

```javascript
    Object {
        key: value
    }
```
在ES5的时代，对象的key只能是字符串String类型。有人就想搞事，把key改成其他数据类型，这不是瞎折腾吗？ES组织的大神们为了对付这类搞事的人，就指定了一个新的数据类型：Symbol。

### 原始数据类型

学习Symbol之前，让我们回忆一下你曾经用过的原始数据类型，只有5个，别搞错了。

**null、undefined**

是不是面试的时候有人问过你这两者的区别？问这种问题的人很无聊，你要是和他当同事，真是受罪。

**Number 数字类型**
```javascript
    const a = 10
    typeof a // number
```

**String 字符串**
```javascript
    const a = 'haha'
    typeof a // string
```

**boolean 布尔型**

```javascript
const a = true, b = false
```

### Symbol

Symbol到底长啥样？又该怎么用呢？我们一起来探索一下。

**在MDN文档中，关于Symbol的说明是这样的：**

Symbol 是一种特殊的、不可变的数据类型，可以作为对象属性的标识符使用。Symbol 对象是一个 symbol primitive data type 的隐式对象包装器。

symbol 数据类型是一个原始数据类型。

**Symbol的语法格式：**

Symbol([description])  //description是可选的

**创建一个Symbol：**

看了Symbol的描述，不知道是什么鬼？长得像个函数。

我们开始按照语法创建一个Symbol来研究一下

```javascript
    const name = Symbol();
    const name1 = Symbol('sym1');
    console.log(name, name1) // Symbol() Symbol(sym1)
```

Symbol不能使用new

```javascript
    const name = new Symbol(); //不可以这样做。
    //Symbol is not a constructor
```

**使用Symbol：**

使用Number的时候，我们可以这样写：

```javascript
    const b = Number(10) // 10
    //简写
    const b = 10
```

同理，使用Symbol，我们可以这样：

```javascript
const name1 = Symbol('sym1'); // Symbol(sym1)
```

在所有使用可计算属性名的地方，都能使用Symbol类型。比如在对象中的key。

```javascript
    const name = Symbol('name');
    const obj = {
      [name]: "haha"
    }
    console.log(obj[name]) // haha
```

你还可以使用Object.defineProperty()和Object.defineProperties()方法。这2个方法是对象的方法，但是作为Symbol类型key，也不影响使用。

```javascript
    // 设置对象属性只读。
    Object.defineProperty(obj, name, {writable: false})
```

这2个方法非常有用，在react源码中，使用了大量的只读属性的对象。以下是从react源码截取的一段代码，设置了props对象只读。但是react仍旧使用字符串作为key，并不用Symbol。

```javascript
    Object.defineProperty(props, 'key', {
        get: warnAboutAccessingKey,
        configurable: true
      });
```

#### Symbol全局共享

Symbol有点特殊，在js文件中定义的Symbol，并不能在其他文件直接共享。

ES6提供了一个注册机制，当你注册Symbol之后，就能在全局共享注册表里面的Symbol。Symbol的注册表和对象表很像，都是key、value结构，只不过这个value是Symbol值。
（key, Symbol）
语法：

```javascript
Symbol.for() //只有一个参数
```


还有一个方法是获取注册表的Symbol。

语法：

```javascript
Symbol.keyFor() //只有一个参数，返回的是key
```

从注册表获取全局共享的Symbol
```javascript
    let name = Symbol.for('name');
    let name1 = Symbol.for('name1');
    let name2 = Symbol.for('name2');
    
    console.log(Symbol.keyFor(name)) // name
    console.log(Symbol.keyFor(name1)) // name1
    console.log(Symbol.keyFor(name2)) // name2
```

注意：如果要防止Symbol命名重复问题，可以加上前缀。如：hyy.name


#### Symbol与类型强制转换

JavaScript中的类型可以自动转换。比如Number转换成字符串。

```javascript
    let a = 1;
    console.log(typeof a); // number
    console.log(a + ' haha') // '1haha'
```

但是注意了，Symbol不支持这种转换。Symbol就是这么拽啊！

```javascript
    let a = Symbol('a');
    console.log(typeof a);
    console.log(a + ' haha') // Cannot convert a Symbol value to a string
```

#### Symbol检索

在对象中获取字符串的key时，可以使用Object.keys()或Object.getOwnPropertyNames()方法获取key，但是使用Symbol做key是，你就只能使用ES6新增的方法来获取了。

```javascript
    let a = Symbol('a');
    let b = Symbol('b');
    
    let obj = {
      [a]: "123",
      [b]: 45
    }
    
    const symbolsKey = Object.getOwnPropertySymbols(obj);
    
    for(let value of symbolsKey) {
      console.log(obj[value]) 
    }
    //"123"
    //45
```

### 总结

Symbol还提供了多个方法给开发者使用，我们不再一一研究每个方法的用途，你想要了解全面可以查看 [Symbol MDN文档][1]

我们只需要知道Symbol如何定义，如何在全局共享，如果在对象中替代key即可应付基本的开发需求了。

最后再回顾一下Symbol是什么：Symbol是JavaScript的原始数据类型，一个全新的数据类型，和对象、数字、字符串等完全不一样，它必须通过Symbol()创建。它的使用看上面的详细介绍。



[1]: https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Symbol
[2]: https://segmentfault.com/a/1190000010199272

上一节：[5.解构：使数据访问更便捷](https://github.com/hyy1115/ES6-learning/blob/master/doc/5%E3%80%81%E3%80%8A%E6%B7%B1%E5%85%A5%E7%90%86%E8%A7%A3ES6%E3%80%8B%E7%AC%94%E8%AE%B0%E2%80%94%E2%80%94%E8%A7%A3%E6%9E%84%EF%BC%9A%E4%BD%BF%E6%95%B0%E6%8D%AE%E8%AE%BF%E9%97%AE%E6%9B%B4%E4%BE%BF%E6%8D%B7.md)

下一节：[7.Set集合与Map集合](https://github.com/hyy1115/ES6-learning/blob/master/doc/7%E3%80%81%E3%80%8A%E6%B7%B1%E5%85%A5%E7%90%86%E8%A7%A3ES6%E3%80%8B%E7%AC%94%E8%AE%B0%E2%80%94%E2%80%94Set%E9%9B%86%E5%90%88%E4%B8%8EMap%E9%9B%86%E5%90%88.md)