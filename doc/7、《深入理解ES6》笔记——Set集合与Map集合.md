Map和Set都叫做集合，但是他们也有所不同。Set常被用来检查对象中是否存在某个键名，Map集合常被用来获取已存的信息。

### Set

#### Set是有序列表，含有相互独立的非重复值。

#### 创建Set

既然我们现在不知道Set长什么样，有什么价值，那么何不创建一个Set集合看看呢？

**创建一个Set集合，你可以这样做：**

```javascript
    let set = new Set();
    console.log(set);
    
    //在浏览器控制台的输出结果
    Set(0) {}
        size:(...)
        __proto__:Set
        [[Entries]]:Array(0)
        length:0
```

看起来像个对象，那么现在我们在控制台打印一个对象，对比一下两者有什么不同。

```javascript
    let obj = new Object()
    console.log(obj)
    
    //在控制台输出对象
    Object {}
        __proto__: 
```

从输出结果看，Set和Object有明显的区别，反正他们就不是一个东西。

**接着，我们看一下Set的原型有哪些：**

![clipboard.png](https://segmentfault.com/img/bVRqlN?w=858&h=652)

这里主要介绍几个基础原型的作用，想要了解全部请前往 [Set集合之家][1] 查看：

Set.prototype.size  
返回Set对象的值的个数。

Set.prototype.add(value)  
在Set对象尾部添加一个元素。返回该Set对象。

Set.prototype.entries()  
返回一个新的迭代器对象，该对象包含Set对象中的按插入顺序排列的所有元素的值的[value, value]数组。为了使这个方法和Map对象保持相似， 每个值的键和值相等。

Set.prototype.forEach(callbackFn[, thisArg])  
按照插入顺序，为Set对象中的每一个值调用一次callBackFn。如果提供了thisArg参数，回调中的this会是这个参数。

Set.prototype.has(value)  
返回一个布尔值，表示该值在Set中存在与否。

**在例子中使用这几个方法测试一下：**

```javascript
    let set = new Set();
    set.add('haha');
    set.add(Symbol('haha'));
    
    console.log(set.size); //2
    
    console.log(set); 
    Set(2) {"haha", Symbol(haha)}
        size:(...)
        __proto__:Set
        [[Entries]]:Array(2)
            0:"haha"
            1:Symbol(haha)
        length:2
        
    console.log(set.has('haha')) // true
```

到这里，你会发现Set像数组，又像一个对象，但又不完全是。

#### 迭代Set

Set既然提供了entries和forEach方法，那么他就是可迭代的。

但如果你使用for in来迭代Set，你不能这样做：

```javascript
    for(let i in sets) {
      console.log(i); //不存在
    }
```
for in迭代的是对象的key，而在Set中的元素没有key，**使用for of来遍历**：

```javascript
    for(let value of sets) {
      console.log(value);
    }
    //"haha"
    //Symbol(haha)
    
    //如果你需要key，则使用下面这种方法
    for(let [key, value] of sets.entries()) {
      console.log(value, key);
    } 
    //"haha" "haha"
    //Symbol(haha) Symbol(haha)
```


**forEach操作Set：**Set本身没有key，而forEach方法中的key被设置成了元素本身。

```javascript
    sets.forEach((value, key) => {
      console.log(value, key);
    });
    //"haha" "haha"
    //Symbol(haha) Symbol(haha)
    
    sets.forEach((value, key) => {
      console.log(Object.is(value, key));
    }); 
    //true true
```

#### Set和Array的转换

Set和数组太像了，Set集合的特点是没有key，没有下标，只有size和原型以及一个可迭代的不重复元素的类数组。既然这样，我们就可以把一个Set集合转换成数组，也可以把数组转换成Set。

```javascript
    //数组转换成Set
    const arr = [1, 2, 2, '3', '3']
    let set = new Set(arr);
    console.log(set) // Set(3) {1, 2, "3"}

    //Set转换成数组
    let set = new Set();
    set.add(1);
    set.add('2');
    console.log(Array.from(set)) // (2) [1, "2"]
```

js面试中，经常会考的一道数组去重题目，就可以使用Set集合的不可重复性来处理。经测试只能去重下面3种类型的数据。

```javascript
    const arr = [1, 1, 'haha', 'haha', null, null]
    let set = new Set(arr);
    console.log(Array.from(set)) // [1, 'haha', null]
    console.log([...set]) // [1, 'haha', null]
```

### Weak Set集合

Set集合本身是强引用，只要new Set()实例化的引用存在，就不释放内存，这样一刀切肯定不好啊，比如你定义了一个DOM元素的Set集合，然后在某个js中引用了该实例，但是当页面关闭或者跳转时，你希望该引用应立即释放内存，Set不听话，那好，你还可以使用 [Weak Set][2]

**语法：**

```javascript
new WeakSet([iterable]);
```

**和Set的区别：**

1、**WeakSet 对象中只能存放对象值**, 不能存放原始值, 而 Set 对象都可以.

2、WeakSet 对象中存储的对象值都是被弱引用的, 如果没有其他的变量或属性引用这个对象值, 则这个对象值会被当成垃圾回收掉. 正因为这样, **WeakSet 对象是无法被枚举的**, 没有办法拿到它包含的所有元素.

**使用：**

```javascript
    let set = new WeakSet();
    const class_1 = {}, class_2 = {};
    set.add(class_1);
    set.add(class_2);
    console.log(set) // WeakSet {Object {}, Object {}}
    console.log(set.has(class_1)) // true
    console.log(set.has(class_2)) // true
```

### Map

#### Map是存储许多键值对的有序列表，key和value支持所有数据类型。

#### 创建Map

如果说Set像数组，那么Map更像对象。而对象中的key只支持字符串，Map更加强大，支持所有数据类型，不管是数字、字符串、Symbol等。

```javascript
    // 一个空Map集合
    let map = new Map()
    console.log(map)
```

![clipboard.png](https://segmentfault.com/img/bVRqxA?w=548&h=188)

**Map的所有原型方法：**
![clipboard.png](https://segmentfault.com/img/bVRqxE?w=874&h=654)

对比Set集合的原型，**Map集合的原型多了set()和get()方法**，注意set()和Set集合不是一个东西。Map没有add，使用set()添加key，value，在Set集合中，使用add()添加value，没有key。

```javascript
    let map = new Map();
    map.set('name', 'haha');
    map.set('id', 10);
    console.log(map)
    // 输出结果
    Map(2) {"name" => "haha", "id" => 10}
        size:(...)
        __proto__:Map
        [[Entries]]:Array(2)
            0:{"name" => "haha"}
            1:{"id" => 10}
        length:2

    console.log(map.get('id')) // 10
    console.log(map.get('name')) // "haha"
```

**使用对象做key**

```javascript
    let map = new Map();
    const key = {};
    map.set(key, '谁知道这是个什么玩意');
    console.log(map.get(key)) // 谁知道这是个什么玩意
```

**Map同样可以使用forEach遍历key、value**

```javascript
    let map = new Map();
    const key = {};
    map.set(key, '这是个什么玩意');
    map.set('name', 'haha');
    map.set('id', 1);
    map.forEach((value, key) => {
      console.log(key, value)
    })
    
    //Object {} "这是个什么玩意"
    //"name" "haha"
    //"id" 1
```

其他Map的使用方法可以前往 [Map之家][3] 学习。

### Weak Map

有强Map，就有弱鸡Map。

和Set要解决的问题一样，希望不再引用Map的时候自动触发垃圾回收机制。那么，你就需要Weak Map。

```javascript
    let map = new WeakMap();
    const key = document.querySelector('.header');
    map.set(key, '这是个什么玩意');
    
    map.get(key) // "这是个什么玩意"
    
    //移除该元素
    key.parentNode.removeChild(key);
    key = null;
```


### 总结

Set集合可以用来过滤数组中重复的元素，只能通过has方法检测指定的值是否存在，或者是通过forEach处理每个值。

Weak Set集合存放对象的弱引用，当该对象的其他强引用被清除时，集合中的弱引用也会自动被垃圾回收机制回收，追踪成组的对象是该集合最好的使用方式。

Map集合通过set()添加键值对，通过get()获取键值，各种方法的使用查看文章教程，你可以把它看成是比Object更加强大的对象。

Weak Map集合只支持对象类型的key，所有key都是弱引用，当该对象的其他强引用被清除时，集合中的弱引用也会自动被垃圾回收机制回收，为那些实际使用与生命周期管理分离的对象添加额外信息是非常适合的使用方式。

记得刚开始学习JavaScript的时候，不知道各种数据类型有什么用，如果你现在刚学习Map和Set也是这种不知道能用来干什么的想法，那么，恭喜，他们已经开始走入你的编程生涯，慢慢的你就会熟悉他们。



[1]: https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Set
[2]: https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/WeakSet
[3]: https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Map
[4]: https://segmentfault.com/a/1190000010199272

上一节：[6.Symbol和Symbol属性](https://github.com/hyy1115/ES6-learning/blob/master/doc/6%E3%80%81%E3%80%8A%E6%B7%B1%E5%85%A5%E7%90%86%E8%A7%A3ES6%E3%80%8B%E7%AC%94%E8%AE%B0%E2%80%94%E2%80%94Symbol%E5%92%8CSymbol%E5%B1%9E%E6%80%A7.md)

下一节：[8.迭代器（Iterator）和生成器（Generator）](https://github.com/hyy1115/ES6-learning/blob/master/doc/8%E3%80%81%E3%80%8A%E6%B7%B1%E5%85%A5%E7%90%86%E8%A7%A3ES6%E3%80%8B%E7%AC%94%E8%AE%B0%E2%80%94%E2%80%94%E8%BF%AD%E4%BB%A3%E5%99%A8%EF%BC%88Iterator%EF%BC%89%E5%92%8C%E7%94%9F%E6%88%90%E5%99%A8%EF%BC%88Generator%EF%BC%89.md)