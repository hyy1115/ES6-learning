相比较于第一章变量的声明，这一章的内容有了一点深度提升，但还不至于很难理解。本章主要讲2个知识点，**字符串**、**正则表达式**

### 字符串
字符串（String）是JavaScript6大原始数据类型。其他几个分别是Boolean、Null、Undefined、Number、Symbol（es6新增）。

字符串类型在前端开发者，是使用最频繁的类型之一，网站上可见的各种文案，几乎都是字符串类型的数据。我们经常需要使用的操作无非是这么几点：读取字符串、转换字符串、清空字符串、拼接字符串、截取字符串。

在ES5中，字符串类型已经有了非常丰富的应用能力，那么，在ES6中，ECMA的专家们对字符串做了什么更新呢？

当Unicode引入扩展字符集之后，16位的字符已经不足以满足字符串的发展，所以才在ES6中更新了Unicode的支持。

我们看看ES6字符串新增的方法

**UTF-16码位：**ES6强制使用UTF-16字符串编码。关于UTF-16的解释请自行百度了解。

**codePointAt()：**
该方法支持UTF-16，接受编码单元的位置而非字符串位置作为参数，返回与字符串中给定位置对应的码位，即一个整数值。

**String.fromCodePoiont()：**作用与codePointAt相反，检索字符串中某个字符的码位，也可以根据指定的码位生成一个字符。

**normalize()**：提供Unicode的标准形式，接受一个可选的字符串参数，指明应用某种Unicode标准形式。


### 正则表达式

**正则表达式u修饰符：**
当给正则表达式添加u字符时，它就从编码单元操作模式切换为字符模式。

### 其他新增的方法

上面提到的字符串和正则的新增方法只有在国际化的时候才用的到，我想，国内的很多网站还是不需要考虑国际化的问题，看不懂就先丢掉。下面讲到的新增的方法是实际开发中需求比较频繁的方法。

**字符串中的子串识别**：

以前我们经常使用indexOf()来检测字符串中是否包含另外一段字符串。

```javascript
    let t = 'abcdefg'
    if(t.indexOf('cde') > -1) {
      console.log(2)
    }
    //输出2，因为t字符串中包含cde字符串。
```

在ES6中，新增了3个新方法。每个方法都接收2个参数，需要检测的子字符串，以及开始匹配的索引位置。

**includes(str, index)：**如果在字符串中检测到指定文本，返回true，否则false。

```javascript
    let t = 'abcdefg'
    if(t.includes('cde')) {
      console.log(2)
    }
    //true
```

**startsWith(str, index)**：如果在字符串起始部分检测到指定文本，返回true，否则返回false。

```javascript
    let t = 'abcdefg'
    if(t.startsWith('ab')) {
      console.log(2)
    }
    //true
```

**endsWith(str, index)**：如果在字符串的结束部分检测到指定文本，返回true，否则返回false。

```javascript
    let t = 'abcdefg'
    if(t.endsWith('fg')) {
      console.log(2)
    }
    //true
```

**如果你只是需要匹配字符串中是否包含某子字符串，那么推荐使用新增的方法，如果需要找到匹配字符串的位置，使用indexOf()。**

**repeat(number)**

这个方法挺有意思的，接收一个Number类型的数据，返回一个重复N次的新字符串。即使这个字符串是空字符，也你能返回N个空字符的新字符串。

```javascript
console.log('ba'.repeat(3)) //bababa
```

### 正则表达式的其他更新

正则表达式y修饰符、正则表达式的复制、flags属性......

由于这一块知识没用过，就不打算去研究实际用途。

### 模板字面量

以前，我们用单引号或双引号表示字符串。
```javascript
let a = '123' //单引号
let b = "123" //双引号
```
现在，使用模板字面量反撇号``。在实际开发中，这是经常都要用到的方法。
```javascript
let c = `123` //反撇号
```
在字符串中使用反撇号，只需要加上转义符。
```javascript
let d = `12\`3` //字符串内插入反撇号的方式。
```

**在多行字符串的使用价值：**

模板字面量为解决多行字符串的一系列问题提供了一个非常好的机制。

如果不使用模板字面量，实现多行字符串，你可能会使用换行符。

```javascript
    let a = '123\n456'
    console.log(a) 
    // 123
    // 456
```

使用模板字面量，就可以非常简单的实现需求。

```javascript
    let a = `123
    456
    `
    console.log(a)
    // 123
    // 456
```

**在模板字面量插入变量的方法。**

我们不再需要使用 +（加号）来向字符串插入变量，而是使用${params}直接插入你需要添加到字符串的位置。

```javascript
    let t = 'haha'
    let a = `123${t}456`
    console.log(a) //123haha456
```

这种方式也叫作字符串占位符。占位符支持互相嵌套模板字面量，强大吧。有了它，我们终于可以抛弃 + 拼接字符串的恶心做法了。

**模板字面量的终极用法**
tag是一个方法，方法名你可以任意命名，这种写法被称作标签模板。

```javascript
    function tag(literals, ...substitutions) {
        //literals是数组，第一个位置是""，第二个位置是占位符之间的字符串，在本例中是haha
        //substitutions是字符串中的模板字面量，可能多个
        
        //函数最终返回字符串
    }
    let a = 4
    let t = tag`${a} haha`
    console.log(t) //4 haha
```

### 总结

本章讲到了ES6中新增的Unicode方法，但是对于不需要做国际化处理的同学，这个功能就不会用到，需要**关注的新增的字符串匹配的3个方法，以及模板字面量的使用。**这2个知识点非常非常常用！

[1]: https://segmentfault.com/a/1190000010199272

上一节：[1.块级作用域绑定](https://github.com/hyy1115/ES6-learning/blob/master/doc/1%E3%80%81%E3%80%8A%E6%B7%B1%E5%85%A5%E7%90%86%E8%A7%A3ES6%E3%80%8B%E7%AC%94%E8%AE%B0%E2%80%94%E2%80%94%E5%9D%97%E7%BA%A7%E4%BD%9C%E7%94%A8%E5%9F%9F%E7%BB%91%E5%AE%9A.md)

下一节：[3.函数](https://github.com/hyy1115/ES6-learning/blob/master/doc/3%E3%80%81%E3%80%8A%E6%B7%B1%E5%85%A5%E7%90%86%E8%A7%A3ES6%E3%80%8B%E7%AC%94%E8%AE%B0%E2%80%94%E2%80%94%E5%87%BD%E6%95%B0.md)