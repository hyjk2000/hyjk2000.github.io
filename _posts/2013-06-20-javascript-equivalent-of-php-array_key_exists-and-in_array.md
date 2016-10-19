---
title: JavaScript 的 array_key_exists 和 in_array
author: James Shih
layout: post
permalink: /2013/06/20/javascript-equivalent-of-php-array_key_exists-and-in_array/
categories:
  - JavaScript
  - Web 开发
tags:
  - array_key_exists
  - in_array
  - JavaScript
---
最近看到网上很多人在问 JavaScript 里有没有像 PHP 里 [`array_key_exists`][1] 和 [`in_array`][2] 那样的函数。答案当然是“没有”，不过网上给出的大部分解决方案都只是遍历检查。我想说，其实还有更简单的办法。

<!--more-->

### array\_key\_exists

在 JavaScript 里实现 `array_key_exists` 是比较简单的，用 [in 运算符][3]：

```javascript
var obj = {
	"Carl": "Old man",
	"Russel": "Little boy",
	"Kevin": "Giant Snipe",
	"Dug": "Golden Retriever"
};

"Kevin" in obj;	// true
"Dug" in obj;	// true
"Alpha" in obj;	// false
```

不过 in 运算符有个值得注意的问题：它不仅会判断一个对象的属性，还会判断它从 prototype chain 继承的属性。如果在判断时需要排除继承来的属性，你需要改用 [`Object.hasOwnProperty`][4] 方法：

```javascript
obj.hasOwnProperty("Carl");		// true
obj.hasOwnProperty("hasOwnProperty");	// false
"hasOwnProperty" in obj;		// true
```

可以看到，`hasOwnProperty` 是继承来的属性，用 `Object.hasOwnProperty` 方法可以判断出它不是 `obj` 自己的属性，而 in 运算符则不会注意到这一点。另外，因为在 JavaScript 中，数组是一种特殊的对象，所以 `Object.hasOwnProperty` 也适用于数组。

BTW，如果你很重视 JavaScript 的性能优化，你就会想到：in 运算符还需要检查对象继承的属性，那么其速度必然较慢。在 Chrome 和 Firefox 上的测试结果表明，[事实的确如此][5]。

### in_array

在 JavaScript 里实现 `in_array` 要麻烦一点，因为数组和对象要分开处理。

对于数组，可以用 [`Array.indexOf`][6] 方法：

```javascript
var arr = ["Carl", "Russel", "Kevin", "Dug"];
arr.indexOf("Russel");	// 1 (返回相应的索引)
arr.indexOf("Beta");	// -1
```

由于 `Array.indexOf` 是 JavaScript 1.6 中新增的，对于 IE 8 及以前的版本，需要一个 fallback：

```javascript
function inArray(needle, haystack) {
	if (indexOf in haystack) {
		return haystack.indexOf(needle) == -1 ? false : true;
	}
	var length = haystack.length;
	for (var i = 0; i < length; i++) {
		if (haystack[i] === needle) return true;
	}
	return false;
}
```

至于对象，则只有遍历一途：

```javascript
function inObject(needle, haystack) {
	for (var i in haystack) {
		if (haystack.hasOwnProperty(i) && haystack[i] === needle) return true;
	}
	return false;
}
```

值得注意的一点：因为用了 [`for...in`][7] 来遍历对象，这同样会遍历到对象继承的属性。所以需要增加 `hasOwnProperty` 的判断，以确保只在对象自己的属性中寻找。

 [1]: http://www.php.net/manual/en/function.array-key-exists.php
 [2]: http://www.php.net/manual/en/function.in-array.php
 [3]: https://developer.mozilla.org/en-US/docs/JavaScript/Reference/Operators/in
 [4]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/hasOwnProperty
 [5]: http://andrew.hedges.name/experiments/in/
 [6]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/indexOf
 [7]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for...in