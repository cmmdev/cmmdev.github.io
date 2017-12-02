---
layout: post
title:  "使用javascript对Monad的一点理解"
date:   2017-11-16 10:20:00 +0800
categories: javascript, functional programming
---

M<T> /// constructor

unit<T>(value: t): M<T>  /// wrapper.

M<T> -> (T -> M<U>) -> M<U>
bind<T, U>(instance: M<T>, transform: (value T) => M(U)) : M<U>  /// transform unwarp to newM

laws:

bind(unit(x), f) === f(x);
bind(m, unit) === m
bind(bind(m, f), g) === bind(m, x=>bind(f(x), g))

bind(unit(x), f) == f(unwarp(unit(x))) = f(x)
bind(m, unit) === unit(unwarp(m)) = m
bind(bind(m, f), g) === g(unwarp(bind(m, f)) = g(unwarp( f(unwarp(m) )))
bind(m, x=>bind(f(x), g)) === bind(f(unwarp(m)), g)  = g(unwarp ( f(unwarp(m)) ))


ref:https://curiosity-driven.org/monads-in-javascript#continuation
ref:https://juejin.im/post/59e55dbbf265da43333d7652
