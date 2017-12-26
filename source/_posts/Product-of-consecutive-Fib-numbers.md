---
title: Product of consecutive Fib numbers
categories: code
date: 2017-05-18 11:23:24
tags: FUNDAMENTALS
---
### Instructions
The Fibonacci numbers are the numbers in the following integer sequence (Fn):
`0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89, 144, 233, ...`

such as

`F(n) = F(n-1) + F(n-2) with F(0) = 0 and F(1) = 1.`

Given a number, say prod (for product), we search two Fibonacci numbers F(n) and F(n+1) verifying

`F(n) * F(n+1) = prod.`

Your function productFib takes an integer (prod) and returns an array:

`[F(n), F(n+1), true] or {F(n), F(n+1), 1} or (F(n), F(n+1), True)`

depending on the language if F(n) * F(n+1) = prod.

If you don't find two consecutive F(m) verifying `F(m) * F(m+1) = prod` you will return

`[F(m), F(m+1), false] or {F(n), F(n+1), 0} or (F(n), F(n+1), False)`

F(m) being the smallest one such as `F(m) * F(m+1) > prod`.

#### Examples
```js
productFib(714) # should return [21, 34, true], 
                # since F(8) = 21, F(9) = 34 and 714 = 21 * 34

productFib(800) # should return [34, 55, false], 
                # since F(8) = 21, F(9) = 34, F(10) = 55 and 21 * 34 < 800 < 34 * 55
```
Notes: Not useful here but we can tell how to choose the number n up to which to go: we can use the "golden ratio" phi which is `(1 + sqrt(5))/2` knowing that F(n) is asymptotic to: `phi^n / sqrt(5)`. That gives a possible upper bound to n.

You can see examples in "Example test".

#### References

[http://en.wikipedia.org/wiki/Fibonacci_number](http://en.wikipedia.org/wiki/Fibonacci_number)

[http://oeis.org/A000045](http://oeis.org/A000045)

#### Sample Tests
```js
Test.assertSimilar(productFib(4895), [55, 89, true])
Test.assertSimilar(productFib(5895), [89, 144, false])
Test.assertSimilar(productFib(74049690), [6765, 10946, true])
Test.assertSimilar(productFib(84049690), [10946, 17711, false])
Test.assertSimilar(productFib(193864606), [10946, 17711, true])
Test.assertSimilar(productFib(447577), [610, 987, false])
Test.assertSimilar(productFib(602070), [610, 987, true])
```

### Normal Solution

```js
function productFib(prod){
  // ...
  let i = 0;

  while(fib(i) * fib(i + 1) < prod) {
    i++;
  }
  
  return [fib(i), fib(i + 1), (fib(i) * fib(i + 1) === prod)];
}

function fib(n){
  if (n === 0) return 0;
  if (n === 1) return 1;
  return fib(n - 1) + fib(n - 2);
}
```

### Elegant Solution

```js
function productFib(prod){
  // ...
  let [m, n] = [0, 1];
  
  while(m * n < prod) {
    [m, n] = [n, m + n];
  }

  return [m, n, m * n === prod];
}
```