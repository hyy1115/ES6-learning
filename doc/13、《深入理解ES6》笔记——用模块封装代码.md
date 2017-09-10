### 模块的定义

模块是自动运行在严格模式下并且没有办法退出运行的JavaScript代码。

模块可以是函数、数据、类，需要指定导出的模块名，才能被其他模块访问。
```javascript
    //数据模块
    const obj = {a: 1}
    //函数模块
    const sum = (a, b) => {
      return a + b
    }
    //类模块
    class My extends React.Components {
    
    }
```
### 模块的导出

给数据、函数、类添加一个export，就能导出模块。一个配置型的JavaScript文件中，你可能会封装多种函数，然后给每个函数加上一个export关键字，就能在其他文件访问到。
```javascript
    //数据模块
    export const obj = {a: 1}
    //函数模块
    export const sum = (a, b) => {
      return a + b
    }
    //类模块
    export class My extends React.Components {
    
    }
```
### 模块的引用

在另外的js文件中，我们可以引用上面定义的模块。使用import关键字，导入分2种情况，一种是导入指定的模块，另外一种是导入全部模块。

1、导入指定的模块。
```javascript
    //导入obj数据，My类
    import {obj, My} from './xx.js'
    
    //使用
    console.log(obj, My)
```
2、导入全部模块
```javascript
    //导入全部模块
    import * as all from './xx.js'
    
    //使用
    console.log(all.obj, all.sun(1, 2), all.My)
```
### 默认模块的使用

如果给我们的模块加上default关键字，那么该js文件默认只导出该模块，你还需要把大括号去掉。
```javascript
    //默认模块的定义
    function sum(a, b) {
        return a + b
    }
    export default sum
    
    //导入默认模块
    import sum from './xx.js'
```
### 模块的使用限制

不能在语句和函数之内使用export关键字，只能在模块顶部使用，作为react和vue开发者的你，这个限制你应该很熟悉了。

**在react中，模块顶部导入其他模块。**
```javascript
    import react from 'react'
```
**在vue中，模块顶部导入其他模块。**
```javascript
    <script>
        import sum from './xx.js'
    </script>
```

### 修改模块导入和导出名

有2种修改方式，一种是模块导出时修改，一种是导入模块时修改。

1、导出时修改：
```javascript
    function sum(a, b) {
        return a + b
    }
    export {sum as add}

    import { add } from './xx.js'
    add(1, 2)
```
2、导入时修改：
```javascript
    function sum(a, b) {
        return a + b
    }
    export sum

    import { sum as add } from './xx.js'
    add(1, 2)
```
### 无绑定导入

当你的模块没有可导出模块，全都是定义的全局变量的时候，你可以使用无绑定导入。

模块：
```javascript
    let a = 1
    const PI = 3.1314
```
无绑定导入：
```javascript
    import './xx.js'
    console.log(a, PI)
```
### 浏览器加载模块

有用过webpack打包js模块的同学可能有经验，使用webpack打包了多个js文件，然后放到HTML使用script加载时，如果加载顺序不对，就会出现找不到模块的错误。

这是因为模块之间是有依赖关系的，就像你使用jQuery的时候，必须先加载jQuery的代码，才能使用jQuery提供的方法。

**加载模块的方法，总是先加载模块1，再加载模块2，因为module类型默认使用defer属性。**
```javascript
    <script type="module" src="module1.js"></script>
    <script type="module" src="module2.js"></script>
```
### 总结

模块还有很多有意思的特性，对react和vue开发有一定经验的人对这些基本知识应该了如指掌，新手不了解也不用太心急，写几个module.js做一下尝试。如果浏览器报错，不能识别export模块，你可能需要先加载babel的js插件来编译它。

[1]: https://segmentfault.com/a/1190000010199272

上一节：[12.代理（Proxy）和反射（Reflection）API](https://github.com/hyy1115/ES6-learning/blob/master/doc/12%E3%80%81%E3%80%8A%E6%B7%B1%E5%85%A5%E7%90%86%E8%A7%A3ES6%E3%80%8B%E7%AC%94%E8%AE%B0%E2%80%94%E2%80%94%E4%BB%A3%E7%90%86%EF%BC%88Proxy%EF%BC%89%E5%92%8C%E5%8F%8D%E5%B0%84%EF%BC%88Reflection%EF%BC%89API.md)

[==>返回目录](https://github.com/hyy1115/ES6-learning/tree/master/doc)
