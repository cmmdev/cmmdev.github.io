---
layout: post
title:  "使用javascript对Monad的一点理解"
date:   2017-11-16 10:20:00 +0800
categories: javascript
---
Monad就是自函子的幺半群。

范畴

范畴（category），作为一个数学概念，是一种包含了对象及对象之间箭头的代数结构。范畴具有两个基本性质：一是对象之间的箭头可以复合，且复合是满足结合律的；二是每个对象到自己有一个单位箭头。一个简单的范畴例子是由集合构成对象，集合间的映射看做箭头。一般来说，对象和箭头可以是抽象的任何类型，范畴的概念提供了一个基本而抽象的方式去研究数学中的对象及其关系的方法。

函子

范畴之间的映射关系。

自函子

相同范畴之间的映射关系

半群

非空集合S，S上定义了二元运算*: S X S -> S（<S, *>称为广群）。若运算*满足结合律，即：
任意a,b,c属于S，有(a * b) * c == a * (b * c) 
则称<S, *>为半群。

幺半群

维基百科中幺半群被定义为是一个伴有二元运算的集合，且这个二元运算只需要满足结合率，并且这个集合中还必须有一个特殊的元素，幺元，对于这个二元运算，一个元素与幺元的运算将返回这个元素自身。 
用公式表示为，假设这个二元运算用 * 表示: 
结合律：对任何在 M 内的a、b、c ， (a * b) * c = a * (b * c) 。 
单位元：存在一在 M 内的元素e，使得任一于 M 内的 a 都会符合 a * e = e * a = a 。 
往往还在满足封闭性，即 a * b 的结果依然在这个集合内。

可以简单的理解Monad就是满足结合律，单位元，封闭性的自映射关系的集合。这个集合的一个元素就是一个Monad实例，
不同的Monad实例之间满足 结合律，单位元，封闭性等性质。 


Monad构造函数：将T类型的对象构造成一个Monad M
```
// T -> M
M<T> 
```

Monad单位元：构造T类型对象为单位元I。
```
// T -> I
unit<T>
```

定义运算Monad实例之间的转换关系 bind: transform unwarp T to new M<U>
```
// M<T> -> (T -> M<U>) -> M<U>
bind(M<T>, T => M<U>)
```

m为某个M的实例，则满足单位元和结合律:
- 单位元：bind(unit(x), M) === M(x);
- 单位元：bind(m, Unit) === m
- 结合律：bind(bind(m, f), g) === bind(m, x=>bind(f(x), g))


定义unwarp为构造函数M对应的解包函数，则存在以下关系：
- bind(unit(x), f) == f(unwarp(unit(x))) = f(x)
- bind(m, unit) === unit(unwarp(m)) = m
- bind(bind(m, f), g) === g(unwarp(bind(m, f)) = g(unwarp( f(unwarp(m) )))
- bind(m, x=>bind(f(x), g)) === bind(f(unwarp(m)), g)  === g(unwarp ( f(unwarp(m)) ))


推荐几篇使用javascript介绍monad的文章:
- [monads-in-javascript][ref-1], 
- [JavaScript 让 Monad 更简单（软件编写）][ref-2]

[ref-1]: https://curiosity-driven.org/monads-in-javascript
[ref-2]: https://juejin.im/post/59e55dbbf265da43333d7652
