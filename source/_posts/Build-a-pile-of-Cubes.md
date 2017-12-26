---
title: Build a pile of Cubes
categories: code
date: 2017-05-17 21:01:41
---
### Instructions
Your task is to construct a building which will be a pile of n cubes. The cube at the bottom will have a volume of n^3, the cube above will have volume of (n-1)^3 and so on until the top which will have a volume of 1^3.

You are given the total volume m of the building. Being given m can you find the number n of cubes you will have to build?

The parameter of the function findNb (find_nb, find-nb, findNb) will be an integer m and you have to return the integer n such as n^3 + (n-1)^3 + ... + 1^3 = m if such a n exists or -1 if there is no such n.

#### Examples

```js
findNb(1071225) --> 45
findNb(91716553919377) --> -1
```

#### Sample Tests

```js
Test.assertEquals(findNb(4183059834009), 2022)
Test.assertEquals(findNb(24723578342962), -1)
Test.assertEquals(findNb(135440716410000), 4824)
Test.assertEquals(findNb(40539911473216), 3568)
```


### Normal Solution

```js
function findNb(m) {
    // your code
    let n, sum = 0;
    
    for (n = 1; sum < m; n++) {
      sum += Math.pow(n, 3);
    }
    if (sum === m) {
      return n - 1;
    }
    
    return (-1);
}
```

### Elegant Solution 1

```js
function findNb(m) {
    // your code
    let n = 0;
    while(m > 0) {
      m -= Math.pow(++n, 3);
    }
    
    return m ? -1 : n;
}
```

### Elegant Solution 2

```js
function findNb(m, n = 0) {
    // your code
    if (m === 0) return n;
    if (m < 0) return -1;
    
    return findNb(m - Math.pow(n + 1, 3), n + 1);
}
```