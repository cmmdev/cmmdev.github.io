---
layout: post
title:  "换硬币／约瑟夫环／八皇后问题的js实现"
date:   2017-11-06 10:20:00 +0800
categories: javascript
---

换硬币方法数：
```
let cost = [0, 1, 5, 10, 50];
// coin counts
function costAt(acc, index) {
  let copy = JSON.parse(JSON.stringify(acc));
  copy[index] ++;
  return copy;
}
let index = 1;

function getKinds(account, num, acc) {
  if (account == 0) {
    console.log('index--' + (index++) );
    console.log(acc);

    return 1;
  } else if (account < 0 || num == 0) {
    // console.log('index--' + (index++) );
    // console.log(acc);
    return 0;
  } else {
    return getKinds(account, num-1, acc) + getKinds(account-cost[num], num, costAt(acc, num))
  }
}

console.log(getKinds(11, 2, [0,0,0,0,0]));
```

约瑟夫环：
```
const R = require("ramda");

let N = 41;
let M = 3;
let K = 1;
let items = R.range(0, N);

// joseph loop
function J(n) {
  if (n <= 1) {
    return 0;
  } else {
    return (J(n-1)+M) % n;
  }
}

console.log(J(41));
```

八皇后问题：
```
// 8Queens

const R = require("ramda");

// (a －> b -> c) -> [a] -> [c]
let map = R.curry(
  (fn, items) => items.map(fn)
);

// [a] -> Bool
let hasSameLength = items =>
  R.reduce(
    (acc, item) => acc = acc && (R.length(item) == R.length(R.head(items))),
    true,
    R.tail(items)
  );

// [a] -> Boolean
let isSafe = arr => {
  return hasSameLength([
    R.compose(
      R.uniq,
      map((item, index) => item + index)
    )(arr),
    R.compose(
      R.uniq,
      map((item, index) => item - index)
    )(arr),
    R.range(0, arr.length)]);
};

// [a] -> a -> [[a]]
let gen = R.curry(
  (items, index) =>
    R.compose(
      R.concat(R.__, [index]),
      R.map(item => item < index ? item : item + 1)
    )(items)
);

// [a] -> a -> [[a]]
let combo = R.curry(
  (items, k) => {
    if (k == 0) return [[0]];

    return R.map(gen(items))(
      R.range(0, k)
    );
  }
);

// a -> [[a]]
let Q = (k) => {
  if (k <= 1) {
    return [[0]];
  } else {
    return R.compose(
      R.reduce(R.concat, []),
      R.map(R.curry(combo)(R.__, k))
    )(Q(k-1))
  }
};

let Q8Answer = R.compose(R.filter(isSafe), Q);

let result = Q8Answer(8);
console.log(result);
console.log(result.length);

```
update: 八皇后问题（backtrace)
```
const R = require('ramda');

// [[[a]]] -> [[a]]
let flatten = R.reduce(R.concat, []);

// [[a]] -> a -> [[[a]]]
let combo = (pres, N) => R.map(
  index => R.map(
    pre => R.concat(pre, [index]),
    pres
  ),
  R.range(0, N)
);

// [[a]] -> a -> [[a]]
let Q = (acc, k, N) => {
  if (k <= 0) return acc;
  else return Q(
    R.compose(
      R.filter(isSafe),
      flatten
    )(combo(acc, N)), k-1, N);
}

console.log(Q([[]], 8, 8).length)
```

