ECMAScript 6 equivalents in ES5

## Arrow Functions

An arrow function expression (also known as fat arrow function) has a shorter syntax compared to function expressions and lexically binds the this value. Arrow functions are always anonymous.


ES6:

```js
[1, 2, 3].map(n => n * 2);
// -> [ 2, 4, 6 ]
```

ES5 equivalent:

```js
[1, 2, 3].map(function(n) { return n * 2; }, this);
// -> [ 2, 4, 6 ]
```

## Block Scoping Functions

Block scoped bindings provide scopes other than the function and top level scope. This ensures your variables don't leak out of the scope they're defined:

ES6:

```js
// let declares a block scope local variable,
// optionally initializing it to a value in ES6

'use strict';

var a = 5;
var b = 10;

if (a === 5) {
  let a = 4; // The scope is inside the if-block
  var b = 1; // The scope is inside the function

  console.log(a);  // 4
  console.log(b);  // 1
}

console.log(a); // 5
console.log(b); // 1
```

ES5:

```js
'use strict';

var a = 5;
var b = 10;

if (a === 5) {
  // technically is more like the following
  (function () {
    var a = 4;
    b = 1;

    console.log(a); // 4
    console.log(b); // 1
  })();
}

console.log(a); // 5
console.log(b); // 1
```


ES6:

```js
'use strict';
// const creates a read-only named constant in ES6.
// define favorite as a constant and give it the value 7
const favorite = 7;
// Attempt to overwrite the constant
try {
  favorite = 15;
} catch (err) {
  console.log('my favorite number is still: ' + favorite);
  // will still print 7
}
```

ES5:

```js
'use strict';
// define favorite as a non-writable `constant` and give it the value 7
Object.defineProperties(window, {
  favorite: {
    value: 7,
    enumerable: true
  }
});
// ^ descriptors are by default false and const are enumerable
var favorite = 7;
// Attempt to overwrite the constant
favorite = 15;
// will still print 7
console.log('my favorite number is still: ' + favorite);
```

## Template Literals

ES6 Template Literals are strings that can include <strong>embedded expressions</strong>. This is sometimes referred to as string interpolation.

ES6:

```js
// Basic usage with an expression placeholder
var person = 'Addy Osmani';
console.log(`Yo! My name is ${person}!`);

// Expressions work just as well with object literals
var user = {name: 'Caitlin Potter'};
console.log(`Thanks for getting this into V8, ${user.name}.`);

// Expression interpolation. One use is readable inline math.
var a = 50;
var b = 100;
console.log(`The number of JS frameworks is ${a + b} and not ${2 * a + b}.`);

// Multi-line strings without needing \n\
console.log(`string text line 1
string text line 2`);

// Functions inside expressions
function fn() { return 'result'; }
console.log(`foo ${fn()} bar`);
```

ES5:

```js
'use strict';

// Basic usage with an expression placeholder
var person = 'Addy Osmani';
console.log('Yo! My name is ' + person + '!');

// Expressions work just as well with object literals
var user = { name: 'Caitlin Potter' };
console.log('Thanks for getting this into V8, ' + user.name + '.');

// Expression interpolation. One use is readable inline math.
var a = 50;
var b = 100;
console.log('The number of JS frameworks is ' + (a + b) + ' and not ' + (2 * a + b) + '.');

// Multi-line strings:
console.log('string text line 1\nstring text line 2');
// Or, alternatively:
console.log('string text line 1\n\
string text line 2');

// Functions inside expressions
function fn() {
  return 'result';
}
console.log('foo ' + fn() + ' bar');
```


## Computed Property Names

Computed property names allow you to specify properties in object literals based on expressions:

ES6:

```js
var prefix = 'foo';
var myObject = {
  [prefix + 'bar']: 'hello',
  [prefix + 'baz']: 'world'
};

console.log(myObject['foobar']);
// -> hello
console.log(myObject['foobaz']);
// -> world
```

ES5:

```js
'use strict';

var prefix = 'foo';
var myObject = {};

myObject[prefix + 'bar'] = 'hello';
myObject[prefix + 'baz'] = 'world';

console.log(myObject['foobar']);
// -> hello
console.log(myObject['foobaz']);
// -> world
```

## Destructuring Assignment

The destructuring assignment syntax is a JavaScript expression that makes it possible to extract data from arrays or objects using a syntax that mirrors the construction of array and object literals.


ES6:

```js
var {foo, bar} = {foo: 'lorem', bar: 'ipsum'};
// foo => lorem and bar => ipsum
```

ES5:

```js
'use strict';

var _ref = { foo: 'lorem', bar: 'ipsum' };

// foo => lorem and bar => ipsum
var foo = _ref.foo;
var bar = _ref.bar;
```

## Default Parameters

Default parameters allow your functions to have optional arguments without needing to check arguments.length or check for undefined.

ES6:

```js
function greet(msg='hello', name='world') {
  console.log(msg,name);
}

greet();
// -> hello world
greet('hey');
// -> hey world
```

ES5:

```js
'use strict';

function greet() {
  // unfair ... if you access arguments[0] like this you can simply
  // access the msg variable name instead
  var msg = arguments[0] === undefined ? 'hello' : arguments[0];
  var name = arguments[1] === undefined ? 'world' : arguments[1];
  console.log(msg, name);
}

function greet(msg, name) {
  (msg === undefined) && (msg = 'hello');
  (name === undefined) && (name = 'world');
  console.log(msg,name);
}

// using basic utility that check against undefined
function greet(msg, name) {
  console.log(
    defaults(msg, 'hello'),
    defaults(name, 'world')
  );
}

greet();
// -> hello world
greet('hey');
// -> hey world
```

ES6:

```js
function f(x, y=12) {
  // y is 12 if not passed (or passed as undefined)
  return x + y;
}

f(3) === 15;
```

ES5:

```js
'use strict';

function f(x, y) {
  if (y === undefined) {
    y = 12;
  }

  return x + y;
}

f(3) === 15;
```

## Iterators And For-Of

Iterators are objects that can traverse a container. It's a useful way to make a class work inside a for of loop.
The interface is similar to the iterators-interface. Iterating with a `for..of` loop looks like:

ES6:

```js
// Behind the scenes, this will get an iterator from the array and loop through it, getting values from it.
for (let element of [1, 2, 3]) {
  console.log(element);
}
// => 1 2 3
```

ES6 (without using `for-of`, if `Symbol` is supported):

```js
'use strict';

for (var _iterator = [1, 2, 3][Symbol.iterator](), _step; !(_step = _iterator.next()).done;) {
  var element = _step.value;
  console.log(element);
}

// => 1 2 3
```

ES5 (approximates):

```js
// Using forEach()
// Doesn't require declaring indexing and element variables in your containing
// scope. They get supplied as arguments to the iterator and are scoped to just
// that iteration.
var a = [1,2,3];
a.forEach(function (element) {
    console.log(element);
});

// => 1 2 3

// Using a for loop
var a = [1,2,3];
for (var i = 0; i < a.length; ++i) {
    console.log(a[i]);
}
// => 1 2 3
```

Note the use of `Symbol`. The ES5 equivalent would require a Symbol polyfill in order to correctly function.

## Classes

This implements class syntax and semantics as described in the ES6 draft spec. Classes are a great way to reuse code.
Several JS libraries provide classes and inheritance, but they aren't mutually compatible.

ES6:

```js
class Hello {
  constructor(name) {
    this.name = name;
  }

  hello() {
    return 'Hello ' + this.name + '!';
  }

  static sayHelloAll() {
    return 'Hello everyone!';
  }
}

class HelloWorld extends Hello {
  constructor() {
    super('World');
  }

  echo() {
    console.log(super.hello());
  }
}

var hw = new HelloWorld();
hw.echo();

console.log(Hello.sayHelloAll());
```

ES5 (approximate):

```js
function Hello(name) {
  this.name = name;
}

Hello.prototype.hello = function hello() {
  return 'Hello ' + this.name + '!';
};

Hello.sayHelloAll = function () {
  return 'Hello everyone!';
};

function HelloWorld() {
  Hello.call(this, 'World');
}

HelloWorld.prototype = Object.create(Hello.prototype);
HelloWorld.prototype.constructor = HelloWorld;
HelloWorld.sayHelloAll = Hello.sayHelloAll;

HelloWorld.prototype.echo = function echo() {
  console.log(Hello.prototype.hello.call(this));
};

var hw = new HelloWorld();
hw.echo();

console.log(Hello.sayHelloAll());
```

A more faithful (albeit, slightly verbose) interpretation can be found in this [Babel](https://goo.gl/ZvEQDq) output.

## Modules

Modules are mostly implemented, with some parts of the Loader API still to be corrected. Modules try to solve many issues in dependencies and deployment, allowing users to create modules with explicit exports, import specific exported names from those modules, and keep these names separate.

*Assumes an environment using CommonJS*


app.js - ES6

```js
import math from 'lib/math';
console.log('2π = ' + math.sum(math.pi, math.pi));
```

app.js - ES5

```js
var math = require('lib/math');
console.log('2π = ' + math.sum(math.pi, math.pi));
```

lib/math.js - ES6

```js
export function sum(x, y) {
  return x + y;
}
export var pi = 3.141593;
```

lib/math.js - ES5

```js
exports.sum = sum;
function sum(x, y) {
  return x + y;
}
var pi = exports.pi = 3.141593;
```

## Property Method Assignment

Method syntax is supported in object initializers, for example see toString():

ES6:

```js
var object = {
  value: 42,
  toString() {
    return this.value;
  }
};

console.log(object.toString() === 42);
// -> true
```

ES5:

```js
'use strict';

var object = {
  value: 42,
  toString: function toString() {
    return this.value;
  }
};

console.log(object.toString() === 42);
// -> true
```

## Object Initializer Shorthand

This allows you to skip repeating yourself when the property name and property value are the same in an object literal.

ES6:

```js
function getPoint() {
  var x = 1;
  var y = 10;

  return {x, y};
}

console.log(getPoint() === {
  x: 1,
  y: 10
});
```

ES5:

```js
'use strict';

function getPoint() {
  var x = 1;
  var y = 10;

  return { x: x, y: y };
}

console.log(getPoint() === {
  x: 1,
  y: 10
});
```

## Rest Parameters

Rest parameters allows your functions to have variable number of arguments without using the arguments object.
The rest parameter is an instance of Array so all the array methods just work.

ES6:

```js
function f(x, ...y) {
  // y is an Array
  return x * y.length;
}

console.log(f(3, 'hello', true) === 6);
// -> true
```

ES5:

```js
'use strict';

function f(x) {
  var y = [];
  y.push.apply(y, arguments) && y.shift();

  // y is an Array
  return x * y.length;
}

console.log(f(3, 'hello', true) === 6);
// -> true
```

## Spread Operator

The spread operator is like the reverse of rest parameters. It allows you to expand an array into multiple formal parameters.

ES6:

```js
function add(a, b) {
  return a + b;
}

let nums = [5, 4];

console.log(add(...nums));
```

ES5:

```js
'use strict';

var _toArray = function (arr) {
  return Array.isArray(arr) ? arr : [].slice.call(arr);
};

function add(a, b) {
  return a + b;
}

var nums = [5, 4];
console.log(add.apply(null, _toArray(nums)));
```

ES6:

```js
function f(x, y, z) {
  return x + y + z;
}
// Pass each elem of array as argument
f(...[1,2,3]) === 6;
```

ES5:

```js
'use strict';

function f(x, y, z) {
  return x + y + z;
}
// Pass each elem of array as argument
f.apply(null, [1, 2, 3]) === 6;
```

## Referencia

* [ECMAScript 6 equivalents in ES5](https://github.com/addyosmani/es6-equivalents-in-es5)
* [ES6 Feature Proposals](http://tc39wiki.calculist.org/es6/)
* [ES6 Features](https://github.com/lukehoban/es6features)
* [ECMAScript 6 support in Mozilla](https://developer.mozilla.org/en-US/docs/Web/JavaScript/New_in_JavaScript/ECMAScript_6_support_in_Mozilla)
* [Babel](https://babeljs.io)
* [JS Rocks](http://jsrocks.org/)
* [Entendendo ES6](http://entendendoes6.com.br)

