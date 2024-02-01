---
title: 'How to use JavaScript array push, unshift, pop, and shift methods'
date: 2022-10-13T00:00:00+05:30
lastmod: 2022-10-13T00:00:00+05:30
draft: false
author: 'Vipin Kumar Madhaan'
authorLink: 'https://ivipin.com/about'
description: 'Guide to write JavaScript array push, unshift, pop, and shift methods'
# featuredImage: '/javascript.svg'
tags: ['JavaScript', 'JS']
categories: ['JavaScript']
lightgallery: true
---

This article primarily introduces the JavaScript array Push, Unshift, Pop, and Shift methods, along with an example form for analyzing the JavaScript array Push, Unshift, Pop, and Shift methods for array addition, deletion, and other related operation skills.

### Push Method

The Push method appends one or more elements to the end of the array and returns the array's new length.
The explanation demonstrates that the push method only needs to add the elements to the end of the array in sequence, without changing the index of the original array elements.

```js
Array.prototype._push = function (...value) {
  for (var i = 0; i < arguments.length; i++) {
    this[this.length] = arguments[i];
  }
  return this.length;
};

var arr = [1, 2, 3, 4];
arr._push(9, 8);
console.log(arr); // [ 1, 2, 3, 4, 9, 8 ]
```

### Unshift Method (Header addition)

The unshift method adds one or more elements to the beginning of the array and returns the new length of the array. The unshift method modifies the original array.
When you use the unshift method to add elements to the array's head, the length of the array changes, but unlike when you add elements to the tail, the original element index of the array does not change. The unshift method simply shifts the index of the original element to the right.

For example, to add just one element, you must shift the index of each array element to the right one by one. Assuming the initial array length is four, adding an element to the head increases the array length to five, and because it moves backward in sequence, its `array[5-1]` must now be the last element.
So we can loop from the last element of the array to the next element, `array[i]` assign the value to the loop, `array[i - 1]` stop at 1, and assign the 0th item of the array to the value that needs to be added.

```js
Array.prototype._unshift = function (value) {
  for (let i = this.length; i > 0; i--) {
    this[i] = this[i - 1];
  }
  this[0] = value;
  return this.length;
};

var arr = [1, 2, 3, 4];
arr._unshift(8);
console.log(arr); // [ 8, 1, 2, 3, 4 ]
```

The above code only adds one element to the head, but the unshift method allows you to add multiple elements. E.G:

```js
var arr = [1, 2, 3, 4];
arr.unshift(8, 7);
console.log(arr); // [ 8, 7, 1, 2, 3, 4 ]
```

In such a case, you should be aware of several parameters that have been entered. You can begin with the arguments object. The concept is as follows: first the loop is based on the length of the last generated array from back to front, moves the elements in the sequence, and then sequentially places the new elements at the head of the array.

The length of the new array is equal to the length of the original array plus the number of parameters. By looping from back to front and moving the final element of the original array to the final element of the new array, The starting point of the loop is the length of the original array plus the number of parameters. Because it is necessary to insert elements with the number of input parameters in the head. The endpoint of the loop is the number of input parameters.
But since the index is always one element less than the length, the start and endpoints need to be decremented by one. Now you can write the logic of circular movement first.

```js
Array.prototype._unshift = function (...value) {
  for (var i = this.length + arguments.length - 1; i > arguments.length - 1; i--) {
    this[i] = this[i - arguments.length];
  }
};
```

In the previous step, the array head's position has been vacated, and the second step is to insert new elements. So you only need to loop through the arguments now.

```js
for (var k = 0; k < arguments.length; k++) {
  this[k] = arguments[k];
}
```

The complete unshift method example:-

```js
Array.prototype._unshift = function(...value) {
  for (var i = (this.length + arguments.length - 1); i > arguments.length - 1; i--) {
    this[i] = this\[i - arguments.length]
  }
  for(var k = 0; k < arguments.length; k++) {
    this[k] = arguments[k]
  }
  return this.length
}

var arr = [1, 2, 3, 4]
arr._unshift(9, 8)
console.log(arr); // [ 9, 8, 1, 2, 3, 4 ]
```

### Pop Method (Tail deletion)

The pop method deletes the last element of the array object, shortens the array by one, and returns the value of the deleted element. If the array is already empty, pop returns an undefined value rather than changing it.
The pop method first saves the last element of the array to facilitate the return, then deletes the last element of the array and sets it to null, and finally determines whether the array is empty. The pop method shortens the array by one element.

```js
Array.prototype._pop = function () {
  if (!this.length) {
    return undefined;
  }
  var end = this[this.length - 1];
  this[this.length - 1] = null;
  this.length = this.length - 1;
  return end;
};

var arr = [1, 2, 3, 4];
arr._pop();
console.log(arr); // [ 1, 2, 3 ]
```

### Shift Method

The shift method is used to delete the first element of the array and return the value of the first element.
The deletion of a header modifies the index of the original array element, causing the index of the elements that have not been deleted to shift to the left. The deleted element must first be recorded for easy return, and then the array's first element is set to null. Finally, the shift method iterates through the array and changes the index for every element.

```js
Array.prototype._shift = function () {
  if (!this.length) {
    return undefined;
  }
  var start = this[0];
  this[0] = null;
  for (var i = 0; i < this.length - 1; i++) {
    this[i] = this[i + 1];
  }
  this.length = this.length - 1;
  return start;
};

var arr = [1, 2, 3, 4];
arr._shift();
console.log(arr); // [ 2, 3, 4 ]
```
