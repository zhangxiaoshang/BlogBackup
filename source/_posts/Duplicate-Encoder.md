---
title: Duplicate Encoder
categories: code
date: 2017-05-21 13:33:43
tags: [FUNDAMENTALS, STRINGS, ARRAYS]
---
### Instructions
The goal of this exercise is to convert a string to a new string where each character in the new string is '(' if that character appears only once in the original string, or ')' if that character appears more than once in the original string. Ignore capitalization when determining if a character is a duplicate.

#### Examples

"din" => "((("

"recede" => "()()()"

"Success" => ")())())"

"(( @" => "))(("

#### Sample Tests

```js
Test.assertEquals(duplicateEncode("din"),"(((");
Test.assertEquals(duplicateEncode("recede"),"()()()");
Test.assertEquals(duplicateEncode("Success"),")())())","should ignore case");
Test.assertEquals(duplicateEncode("(( @"),"))((");
```

### Normal Solution

```js
function duplicateEncode(word){
    // ...
    word = word.toLowerCase();
    let encode = '';
    for(let i in word) {
    	if (word.indexOf(word[i]) == word.lastIndexOf(word[i])) {
    		encode += '(';
    	} else {
    		encode += ')';
    	}
    }
    
    return encode;
}
```

### Elegant Solution

```js
function duplicateEncode(word){
    // ...
    return word
            .toLowerCase()
            .split('')；
            .map(function(c, i, w) {
    	        return w.indexOf(c) === w.lastIndexOf(c) ? '(' : ')';
            })
            .join('');  
}
```

### Clever Solution

```js
function duplicateEncode(word){
    // ...
    return word
    		.toLowerCase()
    		.split('')
    		.map(function(c, i, w) {
    			return w.some(function(c2, i2) {
    				return c2 === c && i2 != i;
    			})  ? ')' : '(';
    		})
    		.join('');
}
```