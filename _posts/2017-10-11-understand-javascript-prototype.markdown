---
layout: post
title:  "理解JavaScript原型链"
date:   2017-10-11 10:20:00 +0800
categories: javascript
---

JavaScript设计之初没有真正的class类，而是使用function当作构造函数的方式来构建对象，
为了以某种简单的方式来实现类的继承关系，而设计了原型继承的方式。

我们用ES6的语法构建一个Animal类和Cat类。
{% highlight ruby %}
class Animal{
    constructor() {}
}

class Cat extends Animal{
    constructor() {
        super()
    }
}

var a = new Animal()
var c = new Cat();
{% endhighlight %}


类（函数），实例对象，原型对象的关系：
prototype 定义的是类（函数）和原型对象的关系，属性的主体是函数，返回值是对象。
例如存在Animal.prototype，而没有a.prototype。
__ proto __ 定义的是对象与原型对象的关系，属性的主体是对象，返回值是对象，
因为Animal是function，是一个特殊的对象，所以同时存在Animal.__ proto __ 和 a.__ proto __。

如何定义Cat和Animal的继承关系呢？（参考babel ES6转译ES5的实现）
Cat的原型对象设置为一个构造函数为Cat的Animal实例，并将Cat.__proto__设置为Animal。
{% highlight ruby %}
Cat.prototype = Object.create(Animal.prototype) 
Cat.prototype.constructor = Cat
Cat.__proto__ = Animal
{% endhighlight %}

实例对象和类有以下关系：
{% highlight ruby %}
Cat.prototype instanceof Cat === false
Cat.prototype instanceof Animal === true
Cat.prototype.constructor === Cat
Cat.__proto__ === Animal

c.__proto__ === Cat.prototype

Animal.prototype.__proto__ === Object.prototype
Animal.__proto__ === Function.prototype
{% endhighlight %}


用大白话讲：
```
//Cat，Animal是类，c，a都是实例。
a.__proto__ === Animal.prototype

//c的原型是Cat.prototype，
c.__proto__ === Cat.prototype

// Cat.prototype是一个Animal实例，所以Cat.prototype的原型是Animal.prototype
c.__proto__.__proto__ === Cat.prototype.__proto__ === (new Animal()).__proto__ === Animal.prototype

// Animal.prototype 是一个普通对象，即Object的实例。所以Animal.prototype的原型是Object.prototype
c.__proto__.__proto__.__proto__ === Animal.prototype.__proto__ === Object.prototype

// Object.prototype 是一个特殊的对象
Object.prototype instanceof Object === false
```

原型链关系如下：
```
x-->y： x的原型是y
实例：c-->Cat.prototype-->Animal.prototype-->Object.prototype-->null
类：Cat-->Animal-->Function.prototype-->Object.prototype-->null
```













