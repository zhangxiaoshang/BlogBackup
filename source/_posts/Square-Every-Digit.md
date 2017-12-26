---
title: Square Every Digit
categories: code
date: 2017-05-20 14:35:28
tags: [FUNDAMENTALS,MATHEMATICS,ALGORITHMS,NUMBERS]
---
### Instructions
Welcome. In this kata, you are asked to square every digit of a number.

For example, if we run 9119 through the function, 811181 will come out.

Note: The function accepts an integer and returns an integer

#### Sample Tests
```js
Test.assertEquals(squareDigits(9119), 811181);
```
### Normal Solution

```js
function squareDigits(num){
  //may the code be with you
  let arr = num.toString().split('');
  let squareArr = arr.map(function(item) {
    return item * item;
  })

  return parseInt(squareArr.join(''));
}
```

### Elegant Solution

```js
function squareDigits(num){
  //may the code be with you
  return Number(num.toString().split('').map(function(i) {return i * i;}).join(''))
}
```