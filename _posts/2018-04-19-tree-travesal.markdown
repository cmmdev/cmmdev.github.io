---
layout: post
title:  "树的遍历算法(js)"
date:   2018-04-19
categories: material
---

```
class Node {
  constructor(val, left, right) {
    this.val = val;
    this.right = right;
    this.left = left;
  }

  toString() {
    return `Node(${this.val})`;
  }
}

function topItem(s) {
  let item = s[s.length - 1];
  return item;
}

// d b e a c f g

function traversalMid(node, visit) {
  var stack = []
  var p = node
  while (p || stack.length > 0) {
    while (p) {
      stack.push(p);
      p = p.left;
    }
    if (stack.length > 0) {
      p = stack.pop();
      visit(p);
      p = p.right
    }
  }
}

function traversalPost(node, visit) {
  var stack = []
  var pre = null;
  var p = null;
  stack.push(node);
  while (stack.length > 0) {

    p = topItem(stack)
    if (
      (!p.left && !p.right) ||
      (pre && (pre == p.left || pre == p.right))) {
      p = stack.pop();
      visit(p)
      pre = p;
    } else {
      if (p.right) stack.push(p.right);
      if (p.left) stack.push(p.left);
    }
  }
}

d = new Node('d', null, null);
e = new Node('e', null, null);
f = new Node('f', null, null);
g = new Node('g', null, null);
b = new Node('b', d, e);
c = new Node('c', f, g);
a = new Node('a', b, c);

traversalPost(a, node=>console.log(node.val))
```



