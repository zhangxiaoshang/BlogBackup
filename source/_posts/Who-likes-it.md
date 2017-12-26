---
title: Who likes it?
date: 2017-05-17 19:52:10
categories: code
---
### Instructions
You probably know the "like" system from Facebook and other pages. People can "like" blog posts, pictures or other items. We want to create the text that should be displayed next to such an item.

Implement a function likes :: [String] -> String, which must take in input array, containing the names of people who like an item. It must return the display text as shown in the examples:

```js
likes [] // must be "no one likes this"
likes ["Peter"] // must be "Peter likes this"
likes ["Jacob", "Alex"] // must be "Jacob and Alex like this"
likes ["Max", "John", "Mark"] // must be "Max, John and Mark like this"
likes ["Alex", "Jacob", "Mark", "Max"] // must be "Alex, Jacob and 2 others like this"
```

### Normal Solution

```js
function likes(names) {
  let len = names.length;
  switch (len) {
    case 0:
      return 'no one likes this';
      break;
     case 1:
       return names[0] + ' likes this';
       break;
     case 2:
       return names[0] + ' and ' + names[1] + ' like this';
       break;
     case 3:
       return names[0] + ', ' + names[1] + ' and ' + names[2] + ' like this';
       break;
     default:
      return names[0] + ', ' + names[1] + ' and ' + (len - 2) + ' others like this';
      break;
  }
}
```

### Elegant Solution

```js
function likes(names) {
  return {
    '0': 'no one likes this',
    '1': `${names[0]}` + ' likes this',
    '2': `${names[0]}` + ' and ' +  `${names[1]}` + ' like this',
    '3': `${names[0]}` + ', ' + `${names[1]}` + ' and ' +  `${names[2]}` + ' like this',
    '4': `${names[0]}` + ', ' + `${names[1]}` + ' and ' +  `${names.length - 2}` + ' others like this'
  }[Math.min(names.length, 4)]
}
```