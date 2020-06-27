---
title: js对象方法，类方法，原型方法
date: 2020-06-22
sidebar: 'auto'
tags:
 - js
categories:
 - js
---

起初是因为在掘金上看到一道关于js对象方法，类方法，原型方法的基础题，感觉自己掌握的不是很好，于是查阅了资料，自己总结了一下

---

## 1.分类定义

```js
    function Person(name){
        this.name = name;
        // 对象方法
        this.talking = function(){
            console.log('调用对象方法')
        }
        // 私有方法
        var jumping = function(){
            console.log('调用私有方法')
        }
    }

    // 类方法
    Person.running = function(){
        console.log('调用类方法')
    }

    // 原型方法
    Person.prototype.writing = function(){
        console.log('调用原型方法')
    }

```

以上定义了一个Person类，其中talking为对象方法，jumping为私有方法，running为类方法，writing为原型方法

>对象方法：构造函数中的方法以及构造函数原型上面的方法，实例化后才可以使用
>类方法：类就是一个函数，在js中由于函数也是一个对象，所以可以为函数添加属性以及方法。
>原型方法：在对象实例话的时候只给对象方法分配内存，而原型方法不需要内存，对性能优化有好处。
>私有方法：只有在内部才能访问到私有方法，外部无法访问到私有方法

## 2.例题
```js
function Person() {
    getAge = function () {
        console.log(10)
    }
    return this;
}

Person.getAge = function () {
    console.log(20)
}

Person.prototype.getAge = function () {
    console.log(30)
}

var getAge = function () {
    console.log(40)
}

function getAge() {
    console.log(50)
}


Person.getAge();            // 20
getAge();                   // 40 
Person().getAge();          // 10
new Person.getAge();        // 20
getAge();                   // 10
new Person().getAge();      // 30

```

Person.getAge()      

> 调用Person的类方法，输出20

getAge()         

> 调用全局getAge()方法，由于变量提升，所以此时getAge()方法输出40

Person().getAge() 

> Person()返回this指向window，之后应调用window下的getAge()方法，但执行Person()时，内部替换了getAge()方法，所以输出10

new Person.getAge()  

> 相当于创建了Person.getAge()的实例，输出20

getAge() 

> 调用window下的getAge()方法，但此前已经将getAge()方法替换，所以输出与第三问相同，为10

new Person().getAge() 

> 创建Person实例对象，但Person并没有定义对象方法getAge()，所以会从原型链中查找getAge()方法，输出30

