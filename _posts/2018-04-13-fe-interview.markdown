---
layout: post
title:  "前端面试资料整理"
date:   2018-04-13
categories: material
---


###### [阿里面试][https://blog.ihoey.com/posts/Interview/2018-02-28-alibaba-interview.html]
###### [前端算法面试题][https://blog.ihoey.com/posts/Interview/2018-02-28-alibaba-interview.html]

###### [佳祥题目]

css:
  1.css水平垂直居中的问题，
  2.有一个div，用js怎么计算这个div到页面左边和顶部的距离，（offsetLeft和offsetTop，）
js:
   j变量提升，把var定义的关键字改成let后变量还会不会提升，
   还有this指向，es6箭头函数的话，this的指向和function定义的函数的this指向有什么不同，变量提升和this
```

    var n = 1;
    function fn() {
        console.log(n)
        var n = 2;
        console.log(n)
    }
    //-----------------------

    var name = 'zhangsan';
    var obj = {
        name: 'lisi',
        fn: function(){
            console.log(this.name)
        }
    }
    obj.fn()
    var fn2 = obj.fn
    fn2()
    // this指向的，fn定义的这个function换成箭头函数后输出会有什么变化，

```



