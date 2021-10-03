---
template: blog-post
title: Optional Chaining Operator
slug: /optional-chaining
date: 2021-10-01
description: Optional Chaining In Javascript.
featuredImage: ../posts/img/chaining.jpg
---

 The idea of optional chaining is to make it easy to write code where you need to access properties or values inside an object or array that may or may not be null/undefined. Let’s take a look with this object `player` :

 ```javascript
player = {
  name: 'Neymar Jr',
  team: 'Paris Saint Germain',
  country: 'Brazil',
  birth: {
    months: 'February',
    years: 1992,
  }
}
```

Let's try to access a value that doesn't exist or is not available.

 ```javascript
console.log(player.number)
```

I'll have an error inside my console, `error: Uncaught ReferenceError: player is not defined` because this value doesn't exist.

Let's imagine we call an API and this value should exist but we have a little problem to access this data, this `error` will break my code.

### The Optional Chaining Operator

The JavaScript Optional Chaining Operator is coming with  ES2020 that allows us to safely access deeply nested properties of an object without having to check for the existence of each of them.

All you have to do is use the “?” operator after the property that you want to check for nullish values. 

```javascript
object?.property
object?.[expression]
```

In our code we can use it like this:

 ```javascript
console.log(player?.number)
```

This will check if the parent value exists and if it does not, it returns undefined and we have no error. When retrieving data from an API, we may not be 100% sure that a certain object exists in our API response.