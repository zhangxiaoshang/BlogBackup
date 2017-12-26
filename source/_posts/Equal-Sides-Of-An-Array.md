---
title: Equal Sides Of An Array
categories: code
date: 2017-05-24 20:30:09
tags: [FUNDAMENTALS, ALGORITHMS, ARRAYS]
---
### Instructions
You are going to be given an array of integers. Your job is to take that array and find an index N where the sum of the integers to the left of N is equal to the sum of the integers to the right of N. If there is no index that would make this happen, return `-1`.

For example:

Let's say you are given the array `{1,2,3,4,3,2,1}`:
Your function will return the index `3`, because at the 3rd position of the array, the sum of left side of the index (`{1,2,3}`) and the sum of the right side of the index (`{3,2,1}`) both equal `6`.

Let's look at another one.
You are given the array `{1,100,50,-51,1,1}`:
Your function will return the index `1`, because at the 1st position of the array, the sum of left side of the index (`{1}`) and the sum of the right side of the index (`{50,-51,1,1}`) both equal `1`.

Note: Please remember that in most programming/scripting languages the index of an array starts at 0.

Input: An integer array of length` 0 < arr < 1000`. The numbers in the array can be any integer positive or negative.

Output: The lowest index `N` where the side to the left of `N` is equal to the side to the right of `N`. If you do not find an index that fits these rules, then you will return `-1`.

Note: If you are given an array with multiple answers, return the lowest correct index.
An empty array should be treated like a `0` in this problem.

#### Examples

#### Sample Tests

```js
    Test.assertEquals(findEvenIndex([1,2,3,4,3,2,1]),3, "The array was: [1,2,3,4,3,2,1] \n");
    Test.assertEquals(findEvenIndex([1,100,50,-51,1,1]),1, "The array was: [1,100,50,-51,1,1] \n");
    Test.assertEquals(findEvenIndex([1,2,3,4,5,6]),-1, "The array was: [1,2,3,4,5,6] \n");
    Test.assertEquals(findEvenIndex([20,10,30,10,10,15,35]),3, "The array was: [20,10,30,10,10,15,35] \n");
```
### Normal Solution

```js
function findEvenIndex(arr)
{
  //Code goes here!
  let sum = arr.reduce(function(acc, val){return acc + val}, 0);
	let left = 0;
  
	for (let i = 0, len = arr.length; i < len; i++) {
		if (left === sum - left - arr[i]) {
			return i;
		} else {
		  left += arr[i];
    }
	}

	return -1;
}
```

### Elegant Solution

```js
function findEvenIndex(arr)
{
  //Code goes here!
  return arr.findIndex(function(element, index, array) {
  	let left = array.slice(0, index).reduce(function(accumulator, element){return accumulator + element;}, 0);
  	let right = array.slice(index + 1).reduce(function(accumulator, element){return accumulator + element;}, 0);

  	return left === right;
  })
}

```

### Es6 Solution

```js
function findEvenIndex(arr){
  //Code goes here!
  return arr.findIndex((element, index, array) => {
  	let left = array
  				    .slice(0, index)
  				    .reduce((accumulator, element) => accumulator + element, 0);
  	let right = array
  				    .slice(index + 1)
  				    .reduce((accumulator, element) => accumulator + element, 0);
    
    return left === right;
  });
}
```