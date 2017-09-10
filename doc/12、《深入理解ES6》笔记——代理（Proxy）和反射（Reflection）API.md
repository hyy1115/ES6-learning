### 反射 Reflect

当你见到一个新的API，不明白的时候，就在浏览器打印出来看看它的样子。

![clipboard.png](https://segmentfault.com/img/bVR5HF?w=1722&h=560)

#### 反射的概念

Reflect 是一个内置的对象，它提供可拦截JavaScript操作的方法。方法与代理处理程序的方法相同。Reflect 不是一个函数对象，因此它是不可构造的。

```javascript
    new Reflect() //错误的写法
```

#### 反射的使用

Reflect提供了一些静态方法，静态方法是指只能通过对象自身访问的的方法，这个知识在前面几章讲解过。**所有方法的详细解析，前往 [Reflect详解][1] 查看。**

**静态方法列表：**这么多静态方法，**你需要学会的是如何使用它们**。

1、Reflect.apply()
2、Reflect.construct()
3、Reflect.defineProperty()
4、Reflect.deleteProperty()
5、Reflect.enumerate()
6、Reflect.get()
7、Reflect.getOwnPropertyDescriptor()
8、Reflect.getPrototypeOf()
9、Reflect.has()
10、Reflect.isExtensible()
11、Reflect.ownKeys()
12、Reflect.preventExtensions()
13、Reflect.set()
14、Reflect.setPrototypeOf()

**静态方法的使用：**

demo1：使用Reflect.get()获取目标对象指定key的value。
```javascript
    let obj = {
        a: 1
    };
    
    let s1 = Reflect.get(obj, "a")
    console.log(s1) // 1
```
demo2:使用Reflect.apply给目标函数floor传入指定的参数。
```javascript
    const s2 = Reflect.apply(Math.floor, undefined, [1.75]); 
    console.log(s2) // 1
```
#### 进一步理解Reflect

看了上面的例子和方法，我们知道Reflect可以拦截JavaScript代码，包括拦截对象，拦截函数等，然后对拦截到的对象或者函数进行读写等操作。

比如demo1的get()方法，拦截obj对象，然后读取key为a的值。当然，不用反射也可以读取a的值。

再看demo2的apply()方法，这个方法你应该比较了解了，和数组中使用apply不同的是，Reflect.apply()提供了3个参数，第一个参数是反射的函数，后面2个参数才是和数组的apply一致。demo2的例子我们可以理解成是拦截了Math.floor方法，然后传入参数，将返回值赋值给s2，这样我们就能在需要读取这个返回值的时候调用s2。
```javascript
    //数组使用apply
    const arr = [1, 2, 3]
    function a() {
      return Array.concat.apply(null, arguments)
    }
    const s = a(arr)
    console.log(s) // [1, 2 ,3]
```

其实Reflect的作用和我们下面要讲的Proxy是差不多的。

### 代理 Proxy

Proxy这个词相信你已经听过无数遍了，我曾经写过一篇webpack使用代理来拦截指定域的API请求，转发到新的目标URL的文章 [webpack中使用proxy][2]。但是注意Proxy和proxy，大小写字母之间是不同的。本章讲的是大写字母开头的Proxy。

![clipboard.png](https://segmentfault.com/img/bVR5W5?w=710&h=182)

#### 语法
```javascript
    let p = new Proxy(target, handler);
```
target：一个目标对象(可以是任何类型的对象，包括本机数组，函数，甚至另一个代理)用Proxy来包装。
handler：一个对象，其属性是当执行一个操作时定义代理的行为的函数。

#### 代理的使用

**基础demo：**Proxy的demo有很多，我们只分析基础demo，主要看new Proxy({}, handler)的操作，指定目标obj对象，然后handler对象执行get()操作，get()返回值的判断是，如果name是target目标对象的属性，则返回target[name]的值，否则返回37，最后测试的时候，p.a是对象p的key，所以返回a的value，而p.b不存在，返回37。
```javascript
    const obj = {
      a: 10
    }
    let handler = {
        get: function(target, name){
            console.log('test: ', target, name)
            // test:  {"a":10} a
            // test:  {"a":10} b
            return name in target ? target[name] : 37
        }
    }
    let p = new Proxy(obj, handler)
    console.log(p.a, p.b) // 10 37
```
这个例子的作用是拦截目标对象obj，当执行obj的读写操作时，进入handler函数进行判断，如果读取的key不存在，则返回默认值。

我们使用一些http-proxy插件或者webpack的时候，有时候需要访问某个api时，跳转到指定的url，这种方式也能解决跨域访问。这种代理模式和Proxy的代理有异曲同工之妙。但是，别混为一体了。
```javascript
    module.exports = {
        devServer: {
           proxy: [
               {
                    context: "/api/*", //代理API
                    target: 'https://www.hyy.com', //目标URL
                    secure: false
              }
           ]
        }
    }
```
### 总结

无论是反射还是代理，除了他们使用方法不同之外，他们所作的事情非常相似，都可以理解成拦截某个东西，然后执行某个函数操作，再返回函数操作的结果。

大部分前端在日常业务需求中，几乎很少使用到这2个API，实际使用场景还得在以后的开发中慢慢挖掘。


[1]: https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Reflect
[2]: https://segmentfault.com/a/1190000008635891
[3]: https://segmentfault.com/a/1190000010199272

上一节：[11.Promise与异步编程](https://github.com/hyy1115/ES6-learning/blob/master/doc/11%E3%80%81%E3%80%8A%E6%B7%B1%E5%85%A5%E7%90%86%E8%A7%A3ES6%E3%80%8B%E7%AC%94%E8%AE%B0%E2%80%94%E2%80%94%20Promise%E4%B8%8E%E5%BC%82%E6%AD%A5%E7%BC%96%E7%A8%8B.md)

下一节：[13.用模块封装代码](https://github.com/hyy1115/ES6-learning/blob/master/doc/13%E3%80%81%E3%80%8A%E6%B7%B1%E5%85%A5%E7%90%86%E8%A7%A3ES6%E3%80%8B%E7%AC%94%E8%AE%B0%E2%80%94%E2%80%94%E7%94%A8%E6%A8%A1%E5%9D%97%E5%B0%81%E8%A3%85%E4%BB%A3%E7%A0%81.md)