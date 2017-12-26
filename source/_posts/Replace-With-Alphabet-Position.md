---
title: Replace With Alphabet Position
categories: code
date: 2017-05-22 11:23:01
tags: [FUNDAMENTALS, STRINGS]
---
### Instructions
Welcome. In this kata you are required to, given a string, replace every letter with its position in the alphabet. If anything in the text isn't a letter, ignore it and don't return it. a being 1, b being 2, etc. As an example:

`alphabet_position("The sunset sets at twelve o' clock.")`
#### Examples

#### Sample Tests

```js
Test.assertEquals(alphabetPosition("The sunset sets at twelve o' clock."), "20 8 5 19 21 14 19 5 20 19 5 20 19 1 20 20 23 5 12 22 5 15 3 12 15 3 11");
Test.assertEquals(alphabetPosition("The narwhal bacons at midnight."), "20 8 5 14 1 18 23 8 1 12 2 1 3 15 14 19 1 20 13 9 4 14 9 7 8 20");
```

### Normal Solution

```js
function alphabetPosition(text) {
	text = text.toLowerCase().replace(/[^a-z]/g, '');
	let arr = [];
	for (let i in text) {
		arr.push(text.charCodeAt(i) - 96)
	}

  return arr.join(' ');
}
```

### Elegant Solution

```js
function alphabetPosition(text) {
	return text
			.toLowerCase()
			.replace(/[^a-z]/g, '')
			.split('')
			.map( (c) => c.charCodeAt() - 96 )
			.join(' ')
}
```