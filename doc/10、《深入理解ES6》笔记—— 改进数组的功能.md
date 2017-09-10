ES5提供的数组已经很强大，但是ES6中继续改进了一些，主要是增加了新的数组方法，所以这章的知识非常少。

### 创建数组
**ES5中创建数组的方式：数组字面量、new一个数组。**

```javascript
    const arr1 = [] //数组字面量
    const arr2 = new Array() //new构建
```

**ES6创建数组：Array.of()、Array.from()**

#### Array.of()

ES5中new一个人数组的时候，会存在一个令人困惑的情况。当new一个数字的时候，生成的是一个长度为该数字的数组，当new一个字符串的时候，生成的是该字符串为元素的数组。

```javascript
    const a = new Array(2)
    const b = new Array("2")
    console.log(a, b) //[undefined, undefined] ["2"]
```

这样一来，导致new Array的行为是不可预测的，Array.of()出现为的就是解决这个情况。

```javascript
    const c = Array.of(2)
    const d = Array.of("2")
    console.log(c, d) // [2] ["2"]
```

使用Array.of()创建的数组传入的参数都是作为数组的元素，而不在是数组长度，这样就避免了使用上的歧义。

#### Array.from()

如果说Array.of()是创建一个新数组，而**Array.from()是将类数组转换成数组**。

下面的例子讲的是将arguments转换成数组。arguments是类数组对象，他表示的是当前函数的所有参数，如果函数没有参数，那么arguments就为空。

```javascript
    function test(a, b) {
      let arr = Array.from(arguments)
      console.log(arr)
    }
    test(1, 2) //[1, 2]
```

**映射转换：**Array.from(arg1, arg2)，我们可以给该方法提供2个参数，第二个参数作为第一个参数的转换。看个简单例子你就懂了。

```javascript
    function test(a, b) {
      let arr = Array.from(arguments, value => value + 2)
      console.log(arr)
    }
    test(1, 2) //[3, 4]
```

Array.from还可以设置第三个参数，指定this。

**Array.from()转换可迭代对象：**这个用法只需要一个例子，数组去重。

```javascript
    function test() {
      return Array.from(new Set(...arguments))
    }
    const s = test([1, "2", 3, 3, "2"])
    console.log(s) // [1,"2",3]
```

### 给数组添加新方法

**ES6给数组添加了几个新方法：find()、findIndex()、fill()、copyWithin()。**

1、find()：传入一个回调函数，找到数组中符合当前搜索规则的第一个元素，返回它，并且终止搜索。

```javascript
    const arr = [1, "2", 3, 3, "2"]
    console.log(arr.find(n => typeof n === "number")) // 1
```

2、findIndex()：传入一个回调函数，找到数组中符合当前搜索规则的第一个元素，返回它的下标，终止搜索。

```javascript
    const arr = [1, "2", 3, 3, "2"]
    console.log(arr.findIndex(n => typeof n === "number")) // 0
```

3、fill()：用新元素替换掉数组内的元素，可以指定替换下标范围。

```javascript
    arr.fill(value, start, end)
```

测试一下

```javascript
    const arr = [1, 2, 3]
    console.log(arr.fill(4)) // [4, 4, 4] 不指定开始和结束，全部替换
    
    const arr1 = [1, 2, 3]
    console.log(arr1.fill(4, 1)) // [1, 4, 4] 指定开始位置，从开始位置全部替换
    
    const arr2 = [1, 2, 3]
    console.log(arr2.fill(4, 0, 2)) // [4, 4, 3] 指定开始和结束位置，替换当前范围的元素
```

4、copyWithin()：选择数组的某个下标，从该位置开始复制数组元素，默认从0开始复制。也可以指定要复制的元素范围。

```javascript
    arr.copyWithin(target, start, end)
```

测试一下
```javascript
    const arr = [1, 2, 3, 4, 5]
    console.log(arr.copyWithin(3)) // [1,2,3,1,2] 从下标为3的元素开始，复制数组，所以4, 5被替换成1, 2
    
    const arr1 = [1, 2, 3, 4, 5]
    console.log(arr1.copyWithin(3, 1)) // [1,2,3,2,3] 从下标为3的元素开始，复制数组，指定复制的第一个元素下标为1，所以4, 5被替换成2, 3
    
    const arr2 = [1, 2, 3, 4, 5]
    console.log(arr2.copyWithin(3, 1, 2)) // [1,2,3,2,5] 从下标为3的元素开始，复制数组，指定复制的第一个元素下标为1，结束位置为2，所以4被替换成2
```

#### 其他新增方法

其他还有定型数组、数组缓冲区的概念，你可以详细查看书上教程。

### 总结

掌握新的创建数组的方式，以及数组新增的几个方法，就够你使用了。定型数组和数组缓冲区一般人可以不用了解的太详细。

[=> 返回文章目录][1]

[1]: https://segmentfault.com/a/1190000010199272

上一节：[9.JavaScript中的类](https://github.com/hyy1115/ES6-learning/blob/master/doc/9%E3%80%81%E3%80%8A%E6%B7%B1%E5%85%A5%E7%90%86%E8%A7%A3ES6%E3%80%8B%E7%AC%94%E8%AE%B0%E2%80%94%E2%80%94%20JavaScript%E4%B8%AD%E7%9A%84%E7%B1%BBclass.md)

下一节：[11.Promise与异步编程](https://github.com/hyy1115/ES6-learning/blob/master/doc/11%E3%80%81%E3%80%8A%E6%B7%B1%E5%85%A5%E7%90%86%E8%A7%A3ES6%E3%80%8B%E7%AC%94%E8%AE%B0%E2%80%94%E2%80%94%20Promise%E4%B8%8E%E5%BC%82%E6%AD%A5%E7%BC%96%E7%A8%8B.md)