---
title: Find the odd int
categories: code
date: 2017-05-17 20:51:32
---
### Instructions
Given an array, find the int that appears an odd number of times.

There will always be only one integer that appears an odd number of times.

### Sample Tests

```js
  doTest([20,1,-1,2,-2,3,3,5,5,1,2,4,20,4,-1,-2,5], 5);
  doTest([1,1,2,-2,5,2,4,4,-1,-2,5], -1);
  doTest([20,1,1,2,2,3,3,5,5,4,20,4,5], 5);
  doTest([10], 10);
  doTest([1,1,1,1,1,1,10,1,1,1,1], 10);
  doTest([5,4,3,2,1,5,4,3,2,10,10], 1);
```

### Normal Solution:

```js
function findOdd(A) {
  //happy coding!
  let result;
  for (let i in A) {
      let count = 0;
      A.forEach(function(num) {
        if (A[i] === num) {
          count++;
        }
      })
      if (count % 2 !== 0) {
      	result = A[i];
      }
  }
  
  return result;
}
```

### Elegant Solution 1

```js
function findOdd(A) {
  //happy coding!
  let set = new Set();
  A.forEach(function(num) {
  	if (set.has(num)) {
  		set.delete(num);
  	} else {
  		set.add(num);
  	}
  })
  let setIter = set.keys();
  
  return setIter.next().value;
}
```

### Elegant Solution 2

```js
function findOdd(A) {
  //happy coding!
  let map = new Map();

  A.forEach(function(num) {
  	if (map.has(num)) {
  		map.delete(num);
  	} else {
  		map.set(num, num);
  	}
  })
  
  let mapIter = map.keys();
  
  return mapIter.next().value;
}
```

### Clever Solution

```js
function findOdd(A) {
  return A.reduce(function(c,v){return c^v;},0);
}
```