# JavaScript Fundamentals --- Notes & Practice Workbook

> **Goal:** Build a reliable foundation in JavaScript before moving on
> to the DOM. Read each concept, type the examples yourself, predict the
> output before running them, and complete the exercises without looking
> at solutions.
>
> **How to use these notes** 1. Read a section and type every example
> into a `.js` file or browser console. 2. Before running code, write
> down what you think it will do. 3. Change values and test edge cases.
> 4. Complete the practice set from memory. 5. Rebuild the mini-projects
> without copying.
>
> Run examples with Node.js (`node filename.js`) or in the browser
> console. These notes cover JavaScript language fundamentals; **DOM
> manipulation is intentionally saved for the next module**.

------------------------------------------------------------------------

## 1. JavaScript variables

Variables are named references used to store values.

``` js
let age = 21;          // can be reassigned
const name = "Raman";  // cannot be reassigned
var city = "Kolkata";  // older function-scoped declaration

age = 22;
// name = "Sam"; // TypeError: assignment to constant variable
```

### `let`, `const`, and `var`

  -----------------------------------------------------------------------
  Feature           `let`             `const`           `var`
  ----------------- ----------------- ----------------- -----------------
  Reassignment      Yes               No                Yes

  Redeclaration in  No                No                Yes
  same scope                                            

  Scope             Block             Block             Function

  Hoisted           Yes,              Yes,              Yes, initialized
                    uninitialized     uninitialized     as `undefined`
                    (TDZ)             (TDZ)             
  -----------------------------------------------------------------------

**Rules** - Prefer `const` by default. - Use `let` when reassignment is
required. - Avoid `var` in modern code. - `const` prevents rebinding; it
does **not** freeze an object or array.

``` js
const user = { name: "Asha" };
user.name = "Mira"; // allowed: object is still the same object
// user = {};       // not allowed: reassigning the binding
```

### Scope and the temporal dead zone

A block is commonly defined by `{}` in `if`, loops, and standalone
blocks.

``` js
{
  let secret = "inside";
  const count = 3;
}
// console.log(secret); // ReferenceError
```

A `let` or `const` variable cannot be accessed before its declaration
executes. This period is the **temporal dead zone (TDZ)**.

------------------------------------------------------------------------

## 2. JavaScript data types

JavaScript has **7 primitive types**:

1.  `string`
2.  `number`
3.  `bigint`
4.  `boolean`
5.  `undefined`
6.  `symbol`
7.  `null`

Everything else is an object type, including arrays and functions.

``` js
const username = "Raman";        // string
const score = 95;                // number
const large = 123456789012345n;  // bigint
const isReady = true;            // boolean
let result;                      // undefined
const uniqueId = Symbol("id");   // symbol
const emptyValue = null;         // null

const skills = ["JS", "Node"];   // array object
const profile = { name: "Raman" }; // object
const greet = () => "Hello";     // function object
```

### Primitive vs non-primitive

-   **Primitives** are immutable values. Assigning one variable to
    another copies the value.
-   **Objects** are mutable. Assigning an object to another variable
    copies the reference, not the object itself.

``` js
let a = 10;
let b = a;
b = 20;
console.log(a); // 10

const first = { points: 10 };
const second = first;
second.points = 50;
console.log(first.points); // 50 — both references point to the same object
```

### `typeof`

``` js
typeof "hello";     // "string"
typeof 42;          // "number"
typeof true;        // "boolean"
typeof undefined;   // "undefined"
typeof 10n;         // "bigint"
typeof Symbol();    // "symbol"
typeof {};          // "object"
typeof [];          // "object"
typeof (() => {});  // "function"
typeof null;        // "object" (historic JavaScript quirk)
```

Use `Array.isArray(value)` to check for arrays.

------------------------------------------------------------------------

## 3. Numbers, `NaN`, and numeric precision

JavaScript's ordinary `number` type uses IEEE 754 double-precision
floating-point numbers. It represents integers exactly only up to
`Number.MAX_SAFE_INTEGER`.

``` js
const price = 99.5;
const count = 4;
console.log(price * count); // 398
console.log(Number.MAX_SAFE_INTEGER);
```

### `NaN`

`NaN` means **Not-a-Number**. It is a numeric value representing an
invalid numeric result.

``` js
const answer = Number("hello");
console.log(answer); // NaN

console.log(Number.isNaN(answer)); // true
console.log(NaN === NaN); // false
```

Use `Number.isNaN()` to test specifically for `NaN`. Global `isNaN()`
first coerces its argument, which can produce surprising results.

### Useful number methods and constants

``` js
Number.isInteger(10);       // true
Number.isFinite(10);        // true
Number.isNaN(NaN);          // true
Math.round(4.6);            // 5
Math.floor(4.9);            // 4
Math.ceil(4.1);             // 5
Math.trunc(4.9);            // 4
Math.max(2, 8, 5);          // 8
Math.min(2, 8, 5);          // 2
Math.random();              // number >= 0 and < 1
Math.abs(-12);              // 12
```

Floating-point arithmetic can be imprecise:

``` js
console.log(0.1 + 0.2); // 0.30000000000000004
console.log((0.1 + 0.2).toFixed(2)); // "0.30" (a string)
```

For money calculations, consider storing the smallest currency unit as
an integer (for example, paise) rather than relying on decimal
floating-point arithmetic.

------------------------------------------------------------------------

## 4. Operators

Operators combine, compare, or transform values.

### Arithmetic operators

  Operator   Meaning                           Example
  ---------- --------------------------------- -----------------
  `+`        Addition / string concatenation   `5 + 2` → `7`
  `-`        Subtraction                       `5 - 2` → `3`
  `*`        Multiplication                    `5 * 2` → `10`
  `/`        Division                          `5 / 2` → `2.5`
  `%`        Remainder                         `5 % 2` → `1`
  `**`       Exponentiation                    `2 ** 3` → `8`
  `++`       Increment by 1                    `x++`
  `--`       Decrement by 1                    `x--`

### Unary operators

A unary operator works with one operand.

``` js
let value = 5;
console.log(+value);       // 5 (numeric conversion)
console.log(-value);       // -5
console.log(++value);      // 6 (increment, then return)
console.log(value++);      // 6 (return, then increment)
console.log(value);        // 7
console.log(typeof value); // "number"
console.log(!true);        // false
console.log(~5);           // -6 (bitwise NOT)
```

Prefer clear standalone increments over complicated expressions mixing
`++` or `--`.

### Assignment operators

``` js
let total = 10;
total += 5; // total = total + 5
total -= 2;
total *= 3;
total /= 2;
total %= 4;
```

### Comparison and relational operators

They return a boolean.

``` js
5 > 3;       // true
5 < 3;       // false
5 >= 5;      // true
5 <= 4;      // false
5 === "5";   // false — strict equality
5 !== "5";   // true
5 == "5";    // true — loose equality with coercion
5 != "5";    // false
```

**Prefer `===` and `!==`.** Loose equality (`==`) performs type coercion
and can be surprising.

``` js
console.log(null == undefined);  // true
console.log(null === undefined); // false
console.log(0 == false);          // true
console.log(0 === false);         // false
```

### Logical operators: AND, OR, NOT

-   `&&` (AND): returns the first falsy operand, or the last operand if
    all are truthy.
-   `||` (OR): returns the first truthy operand, or the last operand if
    all are falsy.
-   `!` (NOT): converts to boolean and reverses truthiness.
-   `??` (nullish coalescing): uses the right side only when the left
    side is `null` or `undefined`.

``` js
true && true;     // true
true && false;    // false
false || "hello"; // "hello"
!true;            // false

const displayName = "" || "Guest";     // "Guest"
const quantity = 0 ?? 10;              // 0
const suppliedName = null ?? "Guest";  // "Guest"
```

Logical operators **short-circuit**: the second expression may not
execute.

``` js
let logged = false;
false && (logged = true);
console.log(logged); // false
```

### Truthy and falsy values

Falsy values are: `false`, `0`, `-0`, `0n`, `""`, `null`, `undefined`,
and `NaN`. All other values are truthy, including `[]` and `{}`.

``` js
if ([]) console.log("Runs"); // Runs
if ({}) console.log("Also runs");
```

### Bitwise operators

Bitwise operators work on 32-bit integer representations of numbers
(with special handling for BigInt). They are not the same as logical
operators.

``` js
5 & 1;  // 1 — bitwise AND
5 | 1;  // 5 — bitwise OR
5 ^ 1;  // 4 — XOR
5 << 1; // 10 — left shift
5 >> 1; // 2 — signed right shift
~5;     // -6 — bitwise NOT
```

Learn what they do, but do not use them as substitutes for ordinary
boolean logic.

------------------------------------------------------------------------

## 5. Booleans and conditional statements

A boolean has only two values: `true` and `false`.

### `if`, `else if`, and `else`

``` js
const marks = 76;

if (marks >= 90) {
  console.log("A");
} else if (marks >= 75) {
  console.log("B");
} else if (marks >= 40) {
  console.log("Pass");
} else {
  console.log("Fail");
}
```

Conditions are evaluated from top to bottom. The first matching branch
runs.

### Ternary operator

Use it for a short expression with two outcomes.

``` js
const age = 20;
const status = age >= 18 ? "Adult" : "Minor";
```

Avoid deeply nested ternaries; use `if/else` for complex logic.

### `switch`

Useful when one expression is compared against several possible values.
Matching uses strict equality.

``` js
const day = 2;

switch (day) {
  case 1:
    console.log("Monday");
    break;
  case 2:
    console.log("Tuesday");
    break;
  case 3:
      console.log("Wednesday");
      break;
  default:
    console.log("Unknown day");
}
```

`break` prevents execution from falling through to the next case.
Intentional fall-through should be clearly documented.

------------------------------------------------------------------------

## 6. Loops

Loops repeat code. Make sure the loop has a condition that eventually
becomes false.

### `for`

Best when the number of iterations or counter is clear.

``` js
for (let i = 1; i <= 5; i++) {
  console.log(i);
}
```

### `while`

Checks the condition before each iteration.

``` js
let i = 1;
while (i <= 5) {
  console.log(i);
  i++;
}
```

### `do...while`

Runs the body at least once, then checks the condition.

``` js
let number = 1;
do {
  console.log(number);
  number++;
} while (number <= 5);
```

### `break` and `continue`

``` js
for (let i = 1; i <= 10; i++) {
  if (i === 3) continue; // skip this iteration
  if (i === 7) break;    // exit the loop
  console.log(i);
}
```

### `for...of` and `for...in`

-   `for...of` iterates over **values** in an iterable (arrays, strings,
    maps, sets).
-   `for...in` iterates over enumerable **property keys** of an object.
    It can include inherited enumerable keys, so use care.

``` js
const colors = ["red", "green", "blue"];
for (const color of colors) {
  console.log(color);
}

const person = { name: "Raman", age: 21 };
for (const key in person) {
  if (Object.hasOwn(person, key)) {
    console.log(key, person[key]);
  }
}
```

For arrays, prefer `for...of`, array methods, or a classic indexed loop
rather than `for...in`.

------------------------------------------------------------------------

## 7. Strings

A string is an immutable sequence of text. String methods return new
strings; they do not modify the original string.

``` js
const firstName = "Raman";
const greeting = "Hello";
```

### Concatenation

``` js
const fullName = firstName + " Patra";
const message = greeting.concat(", ", firstName, "!");
console.log(fullName); // "Raman Patra"
console.log(message);  // "Hello, Raman!"
```

Template literals are usually easier to read:

``` js
const name = "Raman";
const age = 21;
const intro = `My name is ${name} and I am ${age}.`;
```

### Important string properties and methods

  -----------------------------------------------------------------------------------
  Method / property       Purpose                 Example
  ----------------------- ----------------------- -----------------------------------
  `.length`               Number of UTF-16 code   `"hello".length` → `5`
                          units                   

  `.concat()`             Joins strings           `"a".concat("b")` → `"ab"`

  `.includes()`           Checks for substring    `"javascript".includes("script")` →
                                                  `true`

  `.indexOf()`            First matching index,   `"hello".indexOf("l")` → `2`
                          or `-1`                 

  `.slice(start, end)`    Extracts a portion      `"hello".slice(1, 4)` → `"ell"`

  `.split(separator)`     Makes an array          `"a,b".split(",")` → `["a","b"]`

  `.trim()`               Removes edge whitespace `" hi ".trim()` → `"hi"`

  `.trimStart()`          Removes leading         `" hi".trimStart()`
                          whitespace              

  `.trimEnd()`            Removes trailing        `"hi ".trimEnd()`
                          whitespace              

  `.toUpperCase()`        Uppercase copy          `"hi".toUpperCase()`

  `.toLowerCase()`        Lowercase copy          `"HI".toLowerCase()`

  `.replace()`            Replaces first match by `"cat".replace("c","b")` → `"bat"`
                          default                 

  `.replaceAll()`         Replaces all matching   `"a-a".replaceAll("a","b")` →
                          substrings              `"b-b"`

  `.startsWith()`         Checks beginning        `"hello".startsWith("he")`

  `.endsWith()`           Checks ending           `"hello".endsWith("lo")`
  -----------------------------------------------------------------------------------

**Corrections to common terminology** - The method is `.includes()`, not
`.include()`. - JavaScript strings do not have an `.append()` method.
Use `+`, `+=`, template literals, or `.concat()`. - `.join()` is an
**array** method, not a string method.

``` js
const words = "learn javascript well".split(" ");
const sentence = words.join("-");
console.log(sentence); // "learn-javascript-well"
```

### String immutability

``` js
let word = "hello";
word.toUpperCase();
console.log(word); // "hello"

word = word.toUpperCase();
console.log(word); // "HELLO"
```

------------------------------------------------------------------------

## 8. Type conversion

Type conversion changes a value from one type to another.

### Explicit conversion

``` js
String(123);       // "123"
Number("123");     // 123
Number("abc");     // NaN
Boolean(1);        // true
Boolean(0);        // false
Boolean("");       // false
Boolean("false");  // true — non-empty string
```

### `parseInt()` and `parseFloat()`

``` js
parseInt("42", 10);       // 42
parseInt("42px", 10);     // 42
parseFloat("12.75");     // 12.75
parseFloat("12.75kg");   // 12.75

Number("12.75kg");       // NaN — entire string must be numeric
Number("12.75");         // 12.75
```

Pass a radix (usually `10`) to `parseInt()`.

### Conversion to string

``` js
const amount = 250;
const text1 = String(amount);
const text2 = amount.toString();
const text3 = `${amount}`;
```

Be careful: `null` and `undefined` do not have `.toString()` methods,
but `String(null)` and `String(undefined)` work.

### Implicit conversion (coercion)

JavaScript sometimes converts types automatically.

``` js
"5" + 2; // "52" — string concatenation
"5" - 2; // 3 — numeric conversion
"6" * 2; // 12
true + 1; // 2
```

Prefer explicit conversion when it makes the intent clearer.

------------------------------------------------------------------------

## 9. Arrays

An array is an ordered, zero-indexed collection. Arrays can contain
values of different types, though keeping one consistent shape is often
easier to maintain.

``` js
const skills = ["JavaScript", "Node.js", "SQL"];
console.log(skills[0]); // "JavaScript"
console.log(skills.length); // 3
```

Arrays are objects and are mutable.

### Add and remove elements

  ----------------------------------------------------------------------------------------
  Method                                   Action                  Changes original?
  ---------------------------------------- ----------------------- -----------------------
  `push(value)`                            Add to end              Yes

  `pop()`                                  Remove from end         Yes

  `unshift(value)`                         Add to beginning        Yes

  `shift()`                                Remove from beginning   Yes

  `concat(array)`                          Combine arrays          No

  `slice(start, end)`                      Copy a section          No

  `splice(start, deleteCount, ...items)`   Remove/insert in place  Yes

  `includes(value)`                        Check for value         No

  `join(separator)`                        Convert elements to     No
                                           string                  

  `sort(compareFn)`                        Sort in place           Yes

  `reverse()`                              Reverse in place        Yes
  ----------------------------------------------------------------------------------------

``` js
const numbers = [10, 20, 30];
numbers.push(40);       // [10, 20, 30, 40]
numbers.pop();          // removes 40
numbers.unshift(5);     // [5, 10, 20, 30]
numbers.shift();        // removes 5

const more = numbers.concat([40, 50]);
const middle = numbers.slice(1, 3);
const has20 = numbers.includes(20);
const csv = numbers.join(",");
```

### `slice()` vs `splice()`

``` js
const a = ["a", "b", "c", "d"];
const copy = a.slice(1, 3);
console.log(copy); // ["b", "c"]
console.log(a);    // original unchanged

const b = ["a", "b", "c", "d"];
const removed = b.splice(1, 2, "x", "y");
console.log(removed); // ["b", "c"]
console.log(b);       // ["a", "x", "y", "d"]
```

`slice()` uses an exclusive end index. `splice()` changes the original
array.

### Sorting correctly

Default `.sort()` converts elements to strings, so numeric sorting needs
a comparator.

``` js
const values = [100, 5, 20, 3];
values.sort(); // string-like order: not numeric order

const ascending = [100, 5, 20, 3].sort((a, b) => a - b);
const descending = [100, 5, 20, 3].sort((a, b) => b - a);
```

For a non-mutating sort, use `toSorted()` in modern JavaScript:

``` js
const original = [3, 1, 2];
const sortedCopy = original.toSorted((a, b) => a - b);
```

### Copying arrays

``` js
const original = [1, 2, 3];
const copy1 = [...original];
const copy2 = original.slice();
```

These are shallow copies: nested objects are still shared references.

------------------------------------------------------------------------

## 10. Objects

An object stores data as key-value pairs (properties).

``` js
const user = {
  name: "Raman",
  age: 21,
  isStudent: true,
  address: {
    city: "Kolkata",
    state: "West Bengal"
  }
};

console.log(user.name);       // dot notation
console.log(user["age"]);     // bracket notation
console.log(user.address.city);
```

Use bracket notation when the key is dynamic or not a valid identifier.

``` js
const property = "name";
console.log(user[property]);
```

### Add, update, and delete properties

``` js
user.email = "raman@example.com";
user.age = 22;
delete user.isStudent;
```

### Object methods and useful utilities

``` js
const product = {
  name: "Keyboard",
  price: 1200,
  describe() {
    return `${this.name} costs ${this.price}`;
  }
};

console.log(product.describe());

Object.keys(product);    // array of own enumerable string keys
Object.values(product);  // array of values
Object.entries(product); // array of [key, value] pairs
Object.hasOwn(product, "price"); // true
```

`this` in a normal method call refers to the object before the dot.
Arrow functions do not create their own `this`, so use method shorthand
or a normal function when a method needs its receiver as `this`.

``` js
const counter = {
  value: 0,
  increment() {
    this.value++;
  }
};
counter.increment();
console.log(counter.value); // 1
```

### Enhanced object literals (ES6)

Property shorthand and method shorthand reduce repetition.

``` js
const name = "Raman";
const age = 21;

const person = {
  name, // same as name: name
  age,
  greet() {
    return `Hello, I am ${this.name}`;
  }
};

const key = "score";
const result = {
  [key]: 95 // computed property name
};
```

### Object copying and merging

``` js
const base = { name: "Raman", city: "Kolkata" };
const updated = { ...base, city: "Delhi", active: true };
const merged = Object.assign({}, base, { active: true });
```

Spread and `Object.assign()` make shallow copies, not deep clones. For
supported structured data, `structuredClone(value)` can create a deep
clone.

------------------------------------------------------------------------

## 11. Functions

A function is a reusable block of code. It can accept parameters and
return a value.

### Function declaration

``` js
function add(a, b) {
  return a + b;
}
console.log(add(2, 3)); // 5
```

Function declarations are hoisted, so they can generally be called
before their declaration in the same scope.

### Function expression

``` js
const multiply = function (a, b) {
  return a * b;
};
```

### Parameters, arguments, and return

-   **Parameters** are names in the function definition.
-   **Arguments** are actual values passed when calling it.
-   `return` sends a value back and ends that function execution.
-   If there is no explicit `return`, the function returns `undefined`.

``` js
function greet(name = "Guest") {
  return `Hello, ${name}`;
}
greet(); // "Hello, Guest"
```

### Arrow functions

``` js
const square = (number) => number * number;
const sayHello = () => "Hello";
const subtract = (a, b) => {
  const result = a - b;
  return result;
};
```

Arrow functions: - Have shorter syntax. - Do not have their own `this`,
`arguments`, or `prototype`. - Cannot be used as constructors with
`new`. - Are useful for callbacks, but not always suitable for object
methods that need their own `this`.

### Callback functions

A callback is a function passed to another function so it can be called
later.

``` js
function processUser(name, callback) {
  const message = `Hello, ${name}`;
  callback(message);
}

processUser("Raman", function (text) {
  console.log(text);
});
```

Callbacks are common in event handling, timers, array methods, and
asynchronous APIs.

### Global and local scope

``` js
const appName = "StudyApp"; // global/module scope, depending on environment

function showUser() {
  const userName = "Raman"; // local to this function
  console.log(appName, userName);
}

showUser();
// console.log(userName); // ReferenceError
```

JavaScript uses lexical scope: an inner function can access variables in
its outer scope.

``` js
function outer() {
  const message = "I am outside";

  function inner() {
    console.log(message);
  }

  inner();
}
```

### Closures

A closure happens when a function retains access to variables from its
lexical scope even after the outer function has finished.

``` js
function createCounter() {
  let count = 0;

  return function () {
    count++;
    return count;
  };
}

const counter = createCounter();
console.log(counter()); // 1
console.log(counter()); // 2
```

Closures are useful for private state, factories, and callbacks.

------------------------------------------------------------------------

## 12. Spread, rest, and variadic functions

The `...` syntax is used as **spread** or **rest**, depending on where
it appears.

### Spread: expand an iterable or object

``` js
const first = [1, 2];
const second = [3, 4];
const combined = [...first, ...second];

const user = { name: "Raman", city: "Kolkata" };
const updatedUser = { ...user, city: "Delhi" };

console.log(Math.max(...[4, 9, 2])); // 9
```

### Rest: collect remaining values

``` js
function sum(...numbers) {
  return numbers.reduce((total, number) => total + number, 0);
}

console.log(sum(1, 2, 3, 4)); // 10
```

A function that accepts a variable number of arguments is called a
**variadic function**. Rest parameters are a modern way to implement
one.

Only one rest parameter is allowed, and it must be last.

``` js
function introduce(role, ...skills) {
  console.log(role);
  console.log(skills);
}
```

------------------------------------------------------------------------

## 13. Destructuring

Destructuring extracts values from arrays or properties from objects
into variables.

### Array destructuring

``` js
const colors = ["red", "green", "blue"];
const [firstColor, secondColor] = colors;

const [first, , third] = [10, 20, 30];
const [head, ...remaining] = [1, 2, 3, 4];

console.log(firstColor, secondColor); // red green
console.log(first, third);            // 10 30
console.log(remaining);               // [2, 3, 4]
```

Default values:

``` js
const [x = 0, y = 0] = [5];
console.log(x, y); // 5 0
```

### Object destructuring

``` js
const person = { name: "Raman", age: 21, city: "Kolkata" };
const { name, age } = person;
```

Rename a variable and set a default:

``` js
const { name: userName, country = "India" } = person;
```

### Nested destructuring

``` js
const student = {
  name: "Mira",
  address: {
    city: "Kolkata",
    pin: 700001
  }
};

const {
  address: { city, pin }
} = student;
```

### Destructuring in function parameters

This is often what people mean by "function destructuring":
destructuring an object or array passed as a function argument.

``` js
function printUser({ name, age }) {
  console.log(`${name} is ${age}`);
}

printUser({ name: "Raman", age: 21 });
```

With a default object:

``` js
function configure({ theme = "light", debug = false } = {}) {
  console.log(theme, debug);
}
configure();
configure({ debug: true });
```

### Swapping variables

``` js
let a = 1;
let b = 2;
[a, b] = [b, a];
```

------------------------------------------------------------------------

## 14. Array iteration methods

These methods are central to modern JavaScript. Learn what each one
returns and whether it changes the original array.

### `forEach()`

Runs a callback for every element. It returns `undefined`; use it for
side effects, not to build a transformed array.

``` js
const numbers = [1, 2, 3];
numbers.forEach((number, index) => {
  console.log(index, number);
});
```

### `map()`

Creates a new array by transforming each element. Output length is
normally the same as input length.

``` js
const numbers = [1, 2, 3];
const doubled = numbers.map(number => number * 2);
console.log(doubled); // [2, 4, 6]
```

### `filter()`

Creates a new array containing elements whose callback result is truthy.

``` js
const ages = [12, 18, 25, 15];
const adults = ages.filter(age => age >= 18);
console.log(adults); // [18, 25]
```

### `find()`

Returns the first element matching a condition, or `undefined` if none
matches.

``` js
const users = [
  { id: 1, name: "Asha" },
  { id: 2, name: "Raman" }
];

const found = users.find(user => user.id === 2);
```

### `some()` and `every()`

-   `some()` returns `true` if **at least one** element passes.
-   `every()` returns `true` if **all** elements pass.
-   Both return booleans.

``` js
const scores = [70, 85, 40];
scores.some(score => score < 50);  // true
scores.every(score => score >= 40); // true
```

For an empty array, `some()` returns `false` and `every()` returns
`true`.

### `reduce()`

Combines an array into one accumulated result. The callback receives the
accumulator and current element. Supply an initial value in most cases.

``` js
const numbers = [1, 2, 3, 4];

const total = numbers.reduce((accumulator, current) => {
  return accumulator + current;
}, 0);

console.log(total); // 10
```

Build an object with `reduce()`:

``` js
const names = ["Asha", "Mira", "Raman"];
const nameLengths = names.reduce((result, name) => {
  result[name] = name.length;
  return result;
}, {});
```

### Choosing the right method

  Need                                                Use
  --------------------------------------------------- -------------
  Do something for each item (logging, side effect)   `forEach()`
  Transform every item                                `map()`
  Keep matching items                                 `filter()`
  Get the first matching item                         `find()`
  Check whether at least one matches                  `some()`
  Check whether all match                             `every()`
  Accumulate into one result                          `reduce()`

**Common mistake:** `map()` is not a replacement for `forEach()` when
you do not use the returned array. Do not use `filter()` just to find
one item; use `find()`.

------------------------------------------------------------------------

## 15. JSON handling

JSON (JavaScript Object Notation) is a text format commonly used to
exchange structured data. It is not the same thing as a JavaScript
object.

### Convert object to JSON text

``` js
const user = { name: "Raman", age: 21 };
const jsonText = JSON.stringify(user);
console.log(jsonText); // '{"name":"Raman","age":21}'
```

### Convert JSON text to a JavaScript value

``` js
const parsedUser = JSON.parse(jsonText);
console.log(parsedUser.name); // "Raman"
```

### Important points

-   JSON strings require double quotes around property names and string
    values.
-   JSON does not support functions, `undefined`, symbols, or comments.
-   `JSON.stringify()` omits some object properties whose values are
    `undefined`, functions, or symbols.
-   `JSON.parse()` throws a `SyntaxError` for invalid JSON. Use
    `try/catch` for untrusted input.

``` js
try {
  const data = JSON.parse('{"name":"Raman"}');
  console.log(data);
} catch (error) {
  console.error("Invalid JSON:", error.message);
}
```

------------------------------------------------------------------------

## 16. Dates and times

JavaScript's built-in `Date` represents a point in time as milliseconds
since the Unix epoch.

``` js
const now = new Date();
const specificDate = new Date("2026-11-05T10:30:00");
const timestamp = Date.now();
```

### Read date parts

``` js
const date = new Date();

date.getFullYear();
date.getMonth();    // 0 = January, 11 = December
date.getDate();     // day of month
date.getDay();      // 0 = Sunday, 6 = Saturday
date.getHours();
date.getMinutes();
date.getSeconds();
date.getTime();     // milliseconds since epoch
```

### Set date parts

``` js
const date2 = new Date();
date2.setFullYear(2027);
date2.setMonth(0); // January
```

### Format and compare

``` js
const date3 = new Date();
console.log(date3.toISOString()); // UTC ISO string
console.log(date3.toLocaleString()); // locale-dependent display

const earlier = new Date("2026-01-01");
const later = new Date("2026-12-31");
console.log(later > earlier); // true
```

**Time-zone warning:** date-only strings and date-time strings can have
different parsing rules. ISO strings ending in `Z` are UTC. Local
display depends on the machine's time zone. For serious time-zone
scheduling, use a time-zone-aware library or the Temporal API where
supported rather than assuming every day is exactly 24 hours.

------------------------------------------------------------------------

## 17. Timers: `setTimeout()` and `setInterval()`

### `setTimeout()`

Runs a callback once after at least the specified delay (milliseconds).
It is not an exact scheduling guarantee.

``` js
const timeoutId = setTimeout(() => {
  console.log("Runs after about 2 seconds");
}, 2000);

// Cancel before it runs:
clearTimeout(timeoutId);
```

### `setInterval()`

Repeats a callback with an approximate delay between scheduled
executions.

``` js
let count = 0;

const intervalId = setInterval(() => {
  count++;
  console.log(count);

  if (count === 5) {
    clearInterval(intervalId);
  }
}, 1000);
```

The timer callback does not block the rest of the program. Timer delays
can be longer than requested if the event loop is busy or the page is
throttled.

------------------------------------------------------------------------

## 18. ES6 and modern JavaScript essentials

**ES6** (ECMAScript 2015) introduced many features that changed everyday
JavaScript: `let`, `const`, arrow functions, template literals,
destructuring, default parameters, rest/spread syntax, classes, modules,
promises, `Map`, and `Set`.

Some useful modern syntax:

``` js
// Template literal
const message = `Welcome, ${name}`;

// Default parameter
function greet(person = "Guest") {
  return `Hello ${person}`;
}

// Property shorthand
const city = "Kolkata";
const location = { city };

// Optional chaining
const postalCode = user.address?.pin;

// Nullish coalescing
const label = user.nickname ?? "Anonymous";
```

Optional chaining (`?.`) safely stops property access when the value
before it is `null` or `undefined`. It does not hide unrelated errors.

### Modules (intro)

Modules let files share code explicitly.

``` js
// math.js
export function add(a, b) {
  return a + b;
}

// app.js
import { add } from "./math.js";
console.log(add(2, 3));
```

How modules run depends on the environment and project configuration
(for Node.js, configure ES modules appropriately).

------------------------------------------------------------------------

## 19. `Map` data structure

A `Map` stores key-value pairs. Unlike ordinary object property keys,
Map keys can be values of any type, including objects.

``` js
const userRoles = new Map();

userRoles.set("Raman", "admin");
userRoles.set("Mira", "editor");

console.log(userRoles.get("Raman")); // "admin"
console.log(userRoles.has("Mira"));  // true
console.log(userRoles.size);         // 2

userRoles.delete("Mira");
userRoles.clear(); // removes all entries
```

Iterate through a Map:

``` js
const scores = new Map([
  ["Asha", 90],
  ["Raman", 95]
]);

for (const [name, score] of scores) {
  console.log(name, score);
}

scores.keys();
scores.values();
scores.entries();
scores.forEach((value, key) => console.log(key, value));
```

Use `Map` when you need a collection of key-value pairs with arbitrary
key types, frequent additions/removals, or direct iteration.

------------------------------------------------------------------------

## 20. `Set` data structure

A `Set` stores unique values. Adding a duplicate does not create another
entry.

``` js
const ids = new Set();

ids.add(101);
ids.add(102);
ids.add(101);

console.log(ids.size);    // 2
console.log(ids.has(101)); // true

ids.delete(102);
```

Remove duplicates from an array:

``` js
const numbers = [1, 2, 2, 3, 3, 3];
const uniqueNumbers = [...new Set(numbers)];
console.log(uniqueNumbers); // [1, 2, 3]
```

Iterate through a Set:

``` js
const tags = new Set(["js", "node", "js"]);
for (const tag of tags) {
  console.log(tag);
}
```

`Set` preserves insertion order and uses SameValueZero equality for
uniqueness.

------------------------------------------------------------------------

# Practice Workbook

**Instructions:** Do not look up solutions while attempting these. Write
code, test normal and edge cases, and explain your reasoning in plain
language. If stuck, identify the exact concept you are missing, revisit
that section, then try again from a blank file.

## Level 1 --- Variables, types, and operators

1.  Declare a `const` variable for your name, a `let` variable for your
    age, and a boolean for whether you are learning JavaScript. Print
    all three.
2.  Create one value of each of the seven primitive types. Use `typeof`
    on each. Explain the `typeof null` result.
3.  Create an array, an object, and a function. Use `typeof` and
    `Array.isArray()` to inspect them.
4.  Predict and then run: `10 + "5"`, `"10" - 5`, `true + 2`,
    `Boolean("false")`, and `Number("hello")`.
5.  Explain why `NaN === NaN` is false. Show the correct way to test for
    `NaN`.
6.  Swap two variables without creating a third variable.
7.  Given `let x = 5`, show the difference between `x++` and `++x` in
    expressions.
8.  Write expressions using `>`, `>=`, `<`, `<=`, `===`, and `!==`.
9.  Demonstrate one example each of `&&`, `||`, `!`, and `??`. Explain
    the value returned, not just whether it is true or false.
10. Predict the result of `[] || "fallback"`, `0 || 10`, and `0 ?? 10`.
    Explain why.
11. Convert `"450"` to a number, `78.25` to a string, and `"19.8px"` to
    a floating-point number.
12. Write a program that calculates the total price of 3 items, applies
    a percentage discount, and prints the final amount.

## Level 2 --- Conditions and loops

13. Write a program that checks whether a number is positive, negative,
    or zero.
14. Check whether a number is even or odd.
15. Given marks, print a grade using an `if / else if / else` chain.
    Define and handle invalid marks outside 0--100.
16. Build a simple calculator using `switch` for `+`, `-`, `*`, and `/`.
    Handle division by zero and unknown operators.
17. Print numbers from 1 to 100 using a `for` loop.
18. Print all even numbers between 1 and 50.
19. Calculate the sum of numbers from 1 to `n`.
20. Calculate the factorial of a positive integer using a loop. Decide
    how your program handles zero and invalid input.
21. Reverse the digits of a positive integer using a loop.
22. Count the number of digits in an integer without converting it to a
    string.
23. Print a multiplication table for a given number.
24. Recreate one counting loop using `while`, then using `do...while`.
    Explain their difference.
25. Print numbers 1--20 but skip multiples of 3. Stop completely when
    you reach 17.
26. Print this pattern using nested loops:

``` text
*
**
***
****
*****
```

## Level 3 --- Strings

27. Ask for a full name and print it in uppercase, lowercase, and
    trimmed form.
28. Count the number of characters in a string, excluding leading and
    trailing spaces.
29. Count how many times a chosen character appears in a string.
30. Check whether a word contains a given substring using `.includes()`.
31. Convert `"javascript is powerful"` into an array of words, then join
    the words with underscores.
32. Extract the first and last four characters of a string using
    `.slice()`.
33. Capitalize the first letter of a word without changing the remaining
    letters.
34. Check whether a string is a palindrome. Decide how to handle spaces
    and letter case.
35. Replace every space in a sentence with a hyphen.
36. Given `"  Raman,Backend,JavaScript  "`, trim it, split it into
    values, and print each value on its own line.

## Level 4 --- Arrays

37. Create an array of 10 numbers. Print the first, last, and middle
    elements.
38. Add an element at the end and beginning, then remove one from each
    end. Print the array after every operation.
39. Combine two arrays using `.concat()` and spread syntax.
40. Check whether an array contains a particular value.
41. Extract the second through fourth items using `.slice()` without
    changing the original.
42. Use `.splice()` to remove two items and insert a replacement. Show
    both the removed items and modified array.
43. Sort an array of numbers ascending and descending. Explain why the
    default sort is unsuitable.
44. Remove duplicate values from an array using `Set`.
45. Find the largest and smallest values in an array without using
    `Math.max()` or `Math.min()`.
46. Reverse an array manually using a loop (do not use `.reverse()`).
47. Find the second-largest distinct number in an array. Handle arrays
    with too few distinct values.
48. Rotate an array to the right by `k` positions.
49. Merge two already-sorted arrays into one sorted array.
50. Given an array of names, create a new array containing only names
    longer than five characters.

## Level 5 --- Objects and functions

51. Create a `student` object with name, age, marks, and address. Read
    and update properties using both dot and bracket notation.
52. Add a method called `getAverage()` that calculates the average marks
    stored in the object.
53. Print all keys, all values, and all key-value pairs of an object.
54. Create a new object by copying an existing object and changing one
    property with spread syntax. Explain shallow copying.
55. Write a function that accepts two numbers and returns the larger
    one.
56. Write a function that accepts a number and returns whether it is
    prime.
57. Write a function with a default parameter.
58. Convert a function declaration into a function expression and an
    arrow function.
59. Write a callback-based function that accepts a number and a
    callback, then passes the doubled number to the callback.
60. Write a function that returns another function. Use it to create a
    counter.
61. Demonstrate global, function, and block scope with separate
    examples. Predict which variables can be accessed from each
    location.
62. Create an object method that uses `this`. Then rewrite it as an
    arrow function and explain the difference.
63. Write a variadic function that accepts any number of numeric
    arguments and returns their average. Handle zero arguments.
64. Write a function that accepts an object using parameter
    destructuring and default values.

## Level 6 --- Destructuring, spread, rest, and modern syntax

65. Destructure the first, second, and remaining values from an array.
66. Destructure an object's `name` and `email`, renaming `name` to
    `fullName`.
67. Extract a nested city property from an object.
68. Write a function that accepts `{ title, price }` through parameter
    destructuring.
69. Swap two variables with array destructuring.
70. Merge three arrays using spread syntax.
71. Write a function with a rest parameter that returns the maximum
    supplied number.
72. Create an object with property shorthand, a method shorthand, and a
    computed property name.
73. Use a template literal to produce a multi-line profile summary.
74. Use optional chaining to safely read a deeply nested property that
    may not exist.
75. Use `??` to supply a default only when a value is `null` or
    `undefined`; compare it with `||`.

## Level 7 --- Array methods

Use this data for questions 76--84:

``` js
const products = [
  { name: "Keyboard", price: 1200, inStock: true },
  { name: "Mouse", price: 500, inStock: false },
  { name: "Monitor", price: 8500, inStock: true },
  { name: "USB Cable", price: 250, inStock: true }
];
```

76. Use `forEach()` to print every product name and price.
77. Use `map()` to create an array containing only product names.
78. Use `map()` to create a new array with every price increased by 10%.
    Do not mutate the original products.
79. Use `filter()` to get products that are in stock.
80. Use `filter()` to get products that cost more than 1,000.
81. Use `find()` to locate the product named `"Mouse"`.
82. Use `some()` to check whether any product costs more than 5,000.
83. Use `every()` to check whether all products have a positive price.
84. Use `reduce()` to calculate the total price of all products and the
    total price of in-stock products.
85. Given an array of numbers, use `reduce()` to create a
    frequency-count object.
86. Given an array of users, use `find()` to locate a user by ID. What
    should happen if no user exists?
87. Given an array of orders, use `filter()` and `reduce()` to calculate
    the total value of completed orders.
88. For one dataset, solve a task with a loop and then with an array
    method. Explain which version is clearer.

## Level 8 --- JSON, dates, timers, Map, and Set

89. Convert a nested JavaScript object into JSON text and parse it back.
90. Pass invalid JSON to `JSON.parse()` and handle the error using
    `try/catch`.
91. Explain what happens to an object property whose value is
    `undefined` when using `JSON.stringify()`.
92. Create a `Date` object and print year, month, day, hour, and ISO
    string. Remember that month is zero-indexed.
93. Calculate the number of milliseconds between two dates.
94. Write a `setTimeout()` example and cancel it before it executes.
95. Use `setInterval()` to print a counter from 1 to 5, then clear the
    interval.
96. Create a `Map` of student names and scores. Add, read, update,
    delete, and iterate over entries.
97. Create a `Set` of tags. Add duplicates and demonstrate that only
    unique values remain.
98. Convert an array with duplicate IDs into an array of unique IDs
    using `Set`.
99. Use an object as a key in a `Map`. Explain why this differs from
    ordinary object property keys.
100. Explain when you would choose an array, object, Map, or Set.

------------------------------------------------------------------------

# Mini-projects --- prove that you can build without a tutorial

Complete each project in a fresh file. Do not copy a finished solution.
Break the task into small functions and test each function
independently.

## Project 1 --- Student grade manager

Requirements: - Store multiple students as objects in an array. - Each
student has an ID, name, and marks array. - Write functions to calculate
average, grade, pass/fail, and class topper. - Find a student by ID. -
Filter students who passed. - Use `map()` to create a report containing
each student's name and average. - Print a readable report using
template literals. - Handle empty marks and invalid scores.

## Project 2 --- Shopping cart

Requirements: - Store products as objects with ID, name, price, and
stock. - Add a product to the cart and increase quantity if it already
exists. - Remove a product from the cart. - Calculate subtotal using
`reduce()`. - Apply a discount using a function. - Prevent adding more
items than available stock. - Print a formatted bill. - Use `find()`,
`some()`, `map()`, and `reduce()` where appropriate.

## Project 3 --- Expense tracker (console-based)

Requirements: - Store expenses as objects with ID, title, amount,
category, and date. - Add and delete expenses. - Calculate total
spending. - Calculate spending by category using `reduce()`. - Filter
expenses by category and date range. - Use `Map` for category totals. -
Use `Set` to list unique categories. - Serialize the data with
`JSON.stringify()` and parse it back. - Validate amounts and handle an
empty expense list.

## Project 4 --- Quiz engine

Requirements: - Store questions, options, correct answers, and
explanations as objects in an array. - Display one question at a time in
the console. - Accept an answer and validate it. - Track score and
correct/incorrect counts. - Show a final result and percentage. - Use
functions, loops, conditionals, arrays, and objects. - Add a timer for
each question using `setTimeout()` if you want an extra challenge.

------------------------------------------------------------------------

# Self-assessment checklist

Do not move to DOM just because you have read the notes. Move on when
you can do these without copying examples:

-   [ ] Explain `let`, `const`, `var`, primitive values, and references.
-   [ ] Predict type coercion and identify `NaN`, `undefined`, and
    `null`.
-   [ ] Use arithmetic, comparison, logical, unary, and assignment
    operators.
-   [ ] Write conditions and choose between `if`, `switch`, and ternary.
-   [ ] Use `for`, `while`, `do...while`, `for...of`, and `for...in`
    appropriately.
-   [ ] Manipulate strings and arrays, knowing which methods mutate.
-   [ ] Create objects, access properties, and write methods using
    `this`.
-   [ ] Write declarations, expressions, arrow functions, and callbacks.
-   [ ] Explain scope and demonstrate a closure.
-   [ ] Use spread, rest, variadic functions, and destructuring.
-   [ ] Choose correctly among `forEach`, `map`, `filter`, `find`,
    `some`, `every`, and `reduce`.
-   [ ] Parse and stringify JSON safely.
-   [ ] Work with dates, timers, Map, and Set.
-   [ ] Complete at least two mini-projects without following a
    tutorial.

## Suggested revision method

-   **Day 1:** Learn and type the examples. Attempt Level 1--3.
-   **Day 2:** Rebuild the examples from memory. Attempt Level 4--5.
-   **Day 4:** Attempt Level 6--8 without notes.
-   **Day 7:** Build one mini-project from a blank file.
-   **Day 14:** Rebuild a different mini-project and explain your code
    aloud.
-   **Day 30:** Take the full practice set again and record which
    concepts still cause mistakes.

Keep a mistake log with three columns: **What I expected**, **What
actually happened**, and **Why**. Debugging your own incorrect
assumptions is a core JavaScript skill.

------------------------------------------------------------------------

# Before the DOM

When these fundamentals feel comfortable, the next module can cover: 1.
What the DOM is and how HTML becomes a document tree. 2. Selecting
elements (`getElementById`, `querySelector`, `querySelectorAll`). 3.
Reading and changing text, HTML, attributes, classes, and styles. 4.
Creating, inserting, replacing, and removing elements. 5. Events, event
objects, bubbling, capturing, and delegation. 6. Forms, validation, and
user input. 7. DOM mini-projects such as a to-do list, calculator, and
quiz interface.

**Remember:** reading is exposure; writing code from memory, testing
edge cases, and debugging are what build confidence.
