# JavaScript Cheat Sheet

#### Our cheat sheets are designed to help you quickly reference the most commonly used JS techniques

![image](https://blog.logrocket.com/wp-content/uploads/2021/02/machine-learning-libraries-javascript.png)

## Variables
 
    var a;                          // variable
    var b = "init";                 // string
    var c = "Hi" + " " + "Joe";     // = "Hi Joe"
    var d = 1 + 2 + "3";            // = "33"
    var e = [2,3,5,8];              // array
    var f = false;                  // boolean
    var g = /()/;                   // RegEx
    var h = function(){};           // function object
    const PI = 3.14;                // constant
    var a = 1, b = 2, c = a + b;    // one line
    let z = 'zzz';                  // block scope local variable

**Variable Scoping:**
- `var`: Function-scoped, can be redeclared
- `let`: Block-scoped, cannot be redeclared in same scope
- `const`: Block-scoped, cannot be reassigned

___

## Loops

### For Loop

    for (var i = 0; i < 10; i++) {
    document.write(i + ": " + i*3 + "<br />");
    }
    
    var sum = 0;
    for (var i = 0; i < a.length; i++) {
      sum += a[i];
    }               // parsing an array
    
    html = "";
    for (var i of custOrder) {
    html += "<li>" + i + "</li>";
    }

### For...in Loop

    for (var key in object) {
      console.log(key, object[key]);
    }

### For...of Loop

    for (var value of array) {
      console.log(value);
    }

### While Loop

    var i = 1;                      // initialize
    while (i < 100) {               // enters the cycle if statement is true
      i *= 2;                       // increment to avoid infinite loop
      document.write(i + ", ");     // output
    }

### Do...While Loop

    var i = 0;
    do {
      console.log(i);
      i++;
    } while (i < 5);

___

## Conditional Statements

### If - Else

    if ((age >= 14) && (age < 19)) {        // logical condition
      status = "Eligible.";                 // executed if condition is true
    } else {                                // else block is optional
      status = "Not eligible.";             // executed if condition is false
    }

### Ternary Operator

    var status = (age >= 18) ? "Adult" : "Minor";

### Switch Statement

    switch (new Date().getDay()) {      // input is current day
      case 6:                           // if (day == 6)
      text = "Saturday";          
      break;
      case 0:                           // if (day == 0)
      text = "Sunday";
      break;
      default:                          // else...
      text = "Whatever";
    } 

___

## Strict Mode

    "use strict";   // Use strict mode to write secure code
    x = 1;          // Throws an error because variable is not declared

**Benefits:**
- Prevents accidental global variables
- Eliminates `this` coercion
- Disallows duplicate parameter names
- Makes eval() safer

___

## Values and Types

    false, true                     // boolean
    18, 3.14, 0b10011, 0xF6, NaN    // number
    "flower", 'John'                // string
    undefined, null, Infinity       // special

**Type Checking:**

    typeof variable                 // returns type as string
    Array.isArray(variable)         // check if array
    variable instanceof Object      // check instance

___

## Operators

### Arithmetic Operators

    a = b + c - d;      // addition, subtraction
    a = b * (c / d);    // multiplication, division
    x = 100 % 48;       // modulo. 100 / 48 remainder = 4
    a++; b--;           // postfix increment and decrement
    ++a; --b;           // prefix increment and decrement
    a ** b;             // exponentiation (ES2016)

### Comparison Operators

    a == b              // equals (loose equality)
    a != b              // not equal
    a === b             // strict equal (recommended)
    a !== b             // strict not equal (recommended)
    a < b   a > b       // less and greater than
    a <= b  a >= b      // less or equal, greater or equal

### Logical Operators

    a && b              // logical and
    a || b              // logical or
    !a                  // logical not
    a ?? b              // nullish coalescing (ES2020)

### Assignment Operators

    a = b               // assignment
    a += b              // a = a + b (works with - * % **)
    a -= b              // a = a - b
    a *= b              // a = a * b
    a /= b              // a = a / b
    a %= b              // a = a % b

### Other Operators

    a * (b + c)         // grouping
    person.age          // member access
    person[age]         // member access with bracket notation
    typeof a            // type (number, object, function...)
    x << 2  x >> 3      // binary shifting
    x >>> 2             // unsigned right shift
    a ? b : c           // ternary operator
    a ?? b              // nullish coalescing

___

## Strings

    var abc = "abcdefghijklmnopqrstuvwxyz";
    var esc = 'I don\'t \n know';   // \n new line
    var len = abc.length;           // string length
    abc.indexOf("lmno");            // find substring, -1 if doesn't contain 
    abc.lastIndexOf("lmno");        // last occurrence
    abc.slice(3, 6);                // cuts out "def", negative values count from behind
    abc.substring(3, 6);            // similar to slice but doesn't accept negative
    abc.substr(3, 3);               // deprecated, use slice instead
    abc.replace("abc","123");       // find and replace, takes regular expressions
    abc.replaceAll("abc","123");    // replace all occurrences (ES2021)
    abc.toUpperCase();              // convert to upper case
    abc.toLowerCase();              // convert to lower case
    abc.concat(" ", str2);          // abc + " " + str2
    abc.charAt(2);                  // character at index: "c"
    abc[2];                         // unsafe, abc[2] = "C" doesn't work
    abc.charCodeAt(2);              // character code at index: "c" -> 99
    abc.split(",");                 // splitting a string on commas gives an array
    abc.split("");                  // splitting on characters
    128.toString(16);               // number to hex(16), octal (8) or binary (2)
    "Hello".startsWith("He");       // true
    "Hello".endsWith("lo");         // true
    "Hello".includes("ell");         // true
    "  Hello  ".trim();             // "Hello" - removes whitespace
    "Hello".repeat(3);              // "HelloHelloHello"

**Template Literals (ES6):**

    var name = "John";
    var greeting = `Hello, ${name}!`;  // "Hello, John!"
    var multiLine = `Line 1
    Line 2`;

___

## Numbers and Math

    var pi = 3.141;
    pi.toFixed(0);          // returns "3"
    pi.toFixed(2);          // returns "3.14" - for working with money
    pi.toPrecision(2);      // returns "3.1"
    pi.valueOf();           // returns number
    Number(true);           // converts to number: 1
    Number(false);          // converts to number: 0
    Number(new Date())      // number of milliseconds since 1970
    parseInt("3 months");   // returns the first number: 3
    parseFloat("3.5 days"); // returns 3.5
    Number.MAX_VALUE        // largest possible JS number
    Number.MIN_VALUE        // smallest possible JS number
    Number.NEGATIVE_INFINITY// -Infinity
    Number.POSITIVE_INFINITY// Infinity
    Number.isInteger(5);    // true
    Number.isNaN(NaN);      // true
    Number.isFinite(5);     // true

### Math Object

    var pi = Math.PI;       // 3.141592653589793
    Math.round(4.4);        // = 4 - rounded
    Math.round(4.5);        // = 5
    Math.pow(2,8);          // = 256 - 2 to the power of 8
    Math.sqrt(49);          // = 7 - square root 
    Math.abs(-3.14);        // = 3.14 - absolute, positive value
    Math.ceil(3.14);        // = 4 - rounded up
    Math.floor(3.99);       // = 3 - rounded down
    Math.sin(0);            // = 0 - sine
    Math.cos(Math.PI);      // = -1
    Math.tan(0);            // = 0
    Math.atan(1);           // = 0.785... (in radians)
    Math.asin(1);           // = 1.570... (in radians)
    Math.acos(0);           // = 1.570... (in radians)
    Math.min(0, 3, -2, 2);  // = -2 - the lowest value
    Math.max(0, 3, -2, 2);  // = 3 - the highest value
    Math.log(1);            // = 0 natural logarithm 
    Math.exp(1);            // = 2.7182... pow(E,x)
    Math.random();          // random number between 0 and 1
    Math.floor(Math.random() * 5) + 1;  // random integer, from 1 to 5
    Math.sign(-5);          // = -1 (returns sign of number)
    Math.trunc(4.9);        // = 4 (removes decimal part)

___

## Dates

    var d = new Date();
    // Fri Jan 26 2024 02:23:10 GMT+0200 (Eastern European Standard Time)
    // 1706228590110 milliseconds passed since 1970
    Number(d);

### Creating Dates

    new Date();                          // current date and time
    new Date("2017-06-23");             // date declaration
    new Date("2017");                   // is set to Jan 01
    new Date("2017-06-23T12:00:00-09:45");  // date - time YYYY-MM-DDTHH:MM:SSZ
    new Date("June 23 2017");          // long date format
    new Date("Jun 23 2017 07:45:00 GMT+0100 (Tokyo Time)"); // time zone
    new Date(2017, 5, 23);              // year, month (0-11), day
    new Date(2017, 5, 23, 12, 30, 0);   // year, month, day, hour, minute, second

### Getting Date/Time Values

    var d = new Date();
    d.getDate();          // day as a number (1-31)
    d.getDay();           // weekday as a number (0-6)
    d.getFullYear();      // four digit year (yyyy)
    d.getHours();         // hour (0-23)
    d.getMilliseconds();  // milliseconds (0-999)
    d.getMinutes();       // minutes (0-59)
    d.getMonth();         // month (0-11)
    d.getSeconds();       // seconds (0-59)
    d.getTime();          // milliseconds since 1970
    d.getUTCDate();      // UTC day
    d.getUTCFullYear();   // UTC year
    d.getUTCHours();      // UTC hours

### Setting Date/Time Values

    var d = new Date();
    d.setDate(d.getDate() + 7); // adds a week to a date

    d.setDate(15);          // day as a number (1-31)
    d.setFullYear(2024);    // year (optionally month and day)
    d.setHours(12);        // hour (0-23)
    d.setMilliseconds(500); // milliseconds (0-999)
    d.setMinutes(30);      // minutes (0-59)
    d.setMonth(5);         // month (0-11)
    d.setSeconds(45);      // seconds (0-59)
    d.setTime(1706228590110); // milliseconds since 1970

### Date Formatting

    d.toISOString();       // "2024-01-26T00:23:10.110Z"
    d.toLocaleDateString(); // "1/26/2024" (locale-specific)
    d.toLocaleTimeString(); // "12:23:10 AM" (locale-specific)
    d.toString();          // full date string

___

## Arrays

    var dogs = ["Bulldog", "Beagle", "Labrador"]; 
    var dogs = new Array("Bulldog", "Beagle", "Labrador");  // declaration

    alert(dogs[1]);             // access value at index, first item being [0]
    dogs[0] = "Bull Terrier";   // change the first item

    for (var i = 0; i < dogs.length; i++) {     // parsing with array.length
    console.log(dogs[i]);
    }

### Array Methods

    dogs.toString();                        // convert to string: "Bulldog,Beagle,Labrador"
    dogs.join(" * ");                       // join: "Bulldog * Beagle * Labrador"
    dogs.pop();                             // remove last element
    dogs.push("Chihuahua");                 // add new element to the end
    dogs[dogs.length] = "Chihuahua";        // the same as push
    dogs.shift();                           // remove first element
    dogs.unshift("Chihuahua");              // add new element to the beginning
    delete dogs[0];                         // change element to undefined (not recommended)
    dogs.splice(2, 0, "Pug", "Boxer");      // add elements (where, how many to remove, element list)
    dogs.splice(2, 1);                      // remove 1 element at index 2
    var animals = dogs.concat(cats,birds);  // join two arrays (dogs followed by cats and birds)
    dogs.slice(1,4);                        // elements from [1] to [4-1] (doesn't modify original)
    dogs.sort();                            // sort string alphabetically
    dogs.reverse();                         // sort string in descending order
    x.sort(function(a, b){return a - b});   // numeric sort
    x.sort(function(a, b){return b - a});   // numeric descending sort
    highest = x[0];                         // first item in sorted array is the lowest (or highest) value
    x.sort(function(a, b){return 0.5 - Math.random()});     // random order sort

### ES6+ Array Methods

    // Array iteration
    dogs.forEach(function(dog) {
      console.log(dog);
    });
    
    // Map - creates new array
    var lengths = dogs.map(function(dog) {
      return dog.length;
    });
    
    // Filter - creates new array with filtered elements
    var longDogs = dogs.filter(function(dog) {
      return dog.length > 6;
    });
    
    // Find - returns first matching element
    var found = dogs.find(function(dog) {
      return dog.length > 6;
    });
    
    // FindIndex - returns index of first matching element
    var index = dogs.findIndex(function(dog) {
      return dog.length > 6;
    });
    
    // Reduce - reduces array to single value
    var sum = [1, 2, 3, 4].reduce(function(acc, val) {
      return acc + val;
    }, 0);
    
    // Some - returns true if any element passes test
    var hasLong = dogs.some(function(dog) {
      return dog.length > 10;
    });
    
    // Every - returns true if all elements pass test
    var allLong = dogs.every(function(dog) {
      return dog.length > 5;
    });
    
    // Includes - checks if array includes value
    dogs.includes("Beagle");  // true
    
    // Array.from - creates array from array-like object
    var arr = Array.from("Hello");  // ["H", "e", "l", "l", "o"]
    
    // Array destructuring
    var [first, second, ...rest] = dogs;
    
    // Spread operator
    var all = [...dogs, ...cats];

___

## Functions

### Function Declaration

    function myFunction(param1, param2) {
      return param1 + param2;
    }

### Function Expression

    var myFunction = function(param1, param2) {
      return param1 + param2;
    };

### Arrow Functions (ES6)

    var myFunction = (param1, param2) => {
      return param1 + param2;
    };
    
    // Single expression (implicit return)
    var double = x => x * 2;
    
    // No parameters
    var greet = () => "Hello";

### Default Parameters (ES6)

    function greet(name = "Guest") {
      return `Hello, ${name}!`;
    }

### Rest Parameters (ES6)

    function sum(...numbers) {
      return numbers.reduce((a, b) => a + b, 0);
    }

### Function Methods

    function.apply(thisArg, [argsArray]);  // call with array of arguments
    function.call(thisArg, arg1, arg2);    // call with individual arguments
    function.bind(thisArg);                // create new bound function

___

## Objects

### Object Literal

    var person = {
      name: "John",
      age: 30,
      greet: function() {
        return "Hello, " + this.name;
      }
    };
    
    person.name;              // "John"
    person["age"];            // 30
    person.greet();           // "Hello, John"

### Object Methods

    Object.keys(person);       // ["name", "age", "greet"]
    Object.values(person);    // ["John", 30, function]
    Object.entries(person);   // [["name", "John"], ["age", 30], ...]
    Object.assign({}, person); // copy object
    Object.freeze(person);    // prevent modifications
    Object.seal(person);      // prevent adding/deleting properties
    Object.hasOwnProperty("name"); // check if property exists

### ES6 Object Features

    // Shorthand property names
    var name = "John";
    var age = 30;
    var person = { name, age };  // { name: "John", age: 30 }
    
    // Computed property names
    var key = "name";
    var person = { [key]: "John" };  // { name: "John" }
    
    // Method shorthand
    var person = {
      greet() {
        return "Hello";
      }
    };
    
    // Object destructuring
    var { name, age } = person;
    var { name: n, age: a } = person;  // rename
    
    // Spread operator
    var newPerson = { ...person, city: "NYC" };

___

## Classes (ES6)

    class Person {
      constructor(name, age) {
        this.name = name;
        this.age = age;
      }
      
      greet() {
        return `Hello, I'm ${this.name}`;
      }
      
      static createAdult(name) {
        return new Person(name, 18);
      }
    }
    
    class Student extends Person {
      constructor(name, age, school) {
        super(name, age);
        this.school = school;
      }
    }
    
    var john = new Person("John", 30);
    var student = new Student("Jane", 20, "MIT");

___

## Global Functions

    eval();                     // executes a string as if it was script code (avoid using)
    String(23);                 // return string from number: "23"
    (23).toString();            // return string from number: "23"
    Number("23");               // return number from string: 23
    decodeURI(enc);             // decode URI. Result: "my page.asp"
    encodeURI(uri);             // encode URI. Result: "my%20page.asp"
    decodeURIComponent(enc);    // decode a URI component
    encodeURIComponent(uri);    // encode a URI component
    isFinite(5);                // true - is variable a finite, legal number
    isNaN(NaN);                 // true - is variable an illegal number
    parseFloat("3.5");          // returns floating point number: 3.5
    parseInt("23");             // parses a string and returns an integer: 23

___

## Regular Expressions

    var pattern = /CheatSheet/i;
    var result = str.search(pattern);
    var matches = str.match(pattern);
    var replaced = str.replace(pattern, "NewText");

### Modifiers

    i           // perform case-insensitive matching
    g           // perform a global match
    m           // perform multiline matching
    s           // dot matches newline (ES2018)
    u           // unicode mode
    y           // sticky mode

### Patterns

    \           // Escape character
    \d          // find a digit
    \D          // find a non-digit
    \s          // find a whitespace character
    \S          // find a non-whitespace character
    \w          // find a word character (alphanumeric + underscore)
    \W          // find a non-word character
    \b          // find match at beginning or end of a word
    \B          // find match not at word boundary
    n+          // contains at least one n
    n*          // contains zero or more occurrences of n
    n?          // contains zero or one occurrences of n
    ^           // Start of string
    $           // End of string
    \uxxxx      // find the Unicode character
    .           // Any single character (except newline)
    (a|b)       // a or b
    (...)       // Group section
    [abc]       // In range (a, b or c)
    [0-9]       // any of the digits between the brackets
    [^abc]      // Not in range
    a?          // Zero or one of a
    a*          // Zero or more of a
    a*?         // Zero or more, ungreedy
    a+          // One or more of a
    a+?         // One or more, ungreedy
    a{2}        // Exactly 2 of a
    a{2,}       // 2 or more of a
    a{,5}       // Up to 5 of a
    a{2,5}      // 2 to 5 of a
    a{2,5}?     // 2 to 5 of a, ungreedy
    (?=a)       // Positive lookahead
    (?!a)       // Negative lookahead
    (?<=a)      // Positive lookbehind
    (?<!a)      // Negative lookbehind

### RegExp Methods

    pattern.test(str);        // returns true/false
    pattern.exec(str);        // returns match array or null
    str.match(pattern);       // returns match array
    str.search(pattern);      // returns index or -1
    str.replace(pattern, replacement);  // returns new string
    str.split(pattern);       // splits string by pattern

___

## Promises

    function sum(a, b) {
      return new Promise(function (resolve, reject) { 
        setTimeout(function () {
          if (typeof a !== "number" || typeof b !== "number") {
      return reject(new TypeError("Inputs must be numbers"));
      }
      resolve(a + b);
    }, 1000);
    });
    }
    
    var myPromise = sum(10, 5);
    myPromise.then(function (result) {
      console.log("10 + 5:", result);
    return sum(null, "foo");              // Invalid data and return another promise
    }).then(function () {                   // Won't be called because of the error
    }).catch(function (err) {               // The catch handler is called instead
      console.error(err);                   // => TypeError: Inputs must be numbers
    });

### Promise Methods

    Promise.all([p1, p2, p3]);        // waits for all promises
    Promise.allSettled([p1, p2]);     // waits for all (even if rejected)
    Promise.race([p1, p2]);          // returns first settled promise
    Promise.resolve(value);           // returns resolved promise
    Promise.reject(reason);           // returns rejected promise

___

## Async/Await (ES2017)

    async function fetchData() {
      try {
        const response = await fetch('https://api.example.com/data');
        const data = await response.json();
        return data;
      } catch (error) {
        console.error('Error:', error);
        throw error;
      }
    }
    
    // Using async function
    fetchData().then(data => console.log(data));

___

## Error Handling

    try {
      // code that may throw error
      riskyFunction();
    } catch (error) {
      // handle error
      console.error(error.message);
    } finally {
      // always executes
      cleanup();
    }
    
    // Throwing errors
    throw new Error("Something went wrong");
    throw new TypeError("Invalid type");
    throw new ReferenceError("Variable not defined");

___

## Modules (ES6)

### Export

    // Named export
    export function myFunction() {}
    export const myConstant = 42;
    export class MyClass {}
    
    // Default export
    export default function() {}
    export default class MyClass {}
    
    // Export list
    export { myFunction, myConstant };

### Import

    // Named import
    import { myFunction, myConstant } from './module.js';
    
    // Default import
    import myDefault from './module.js';
    
    // Import all
    import * as module from './module.js';
    
    // Rename import
    import { myFunction as func } from './module.js';
    
    // Mixed
    import myDefault, { named } from './module.js';

___

## Destructuring

### Array Destructuring

    var [a, b, c] = [1, 2, 3];
    var [a, ...rest] = [1, 2, 3, 4];  // rest = [2, 3, 4]
    var [a, b = 10] = [1];            // b = 10 (default)

### Object Destructuring

    var {name, age} = {name: "John", age: 30};
    var {name: n, age: a} = {name: "John", age: 30};  // rename
    var {name, ...rest} = {name: "John", age: 30, city: "NYC"};

___

## Spread Operator

    // Arrays
    var arr1 = [1, 2, 3];
    var arr2 = [...arr1, 4, 5];  // [1, 2, 3, 4, 5]
    
    // Objects
    var obj1 = {a: 1, b: 2};
    var obj2 = {...obj1, c: 3};  // {a: 1, b: 2, c: 3}
    
    // Function arguments
    function sum(a, b, c) { return a + b + c; }
    sum(...[1, 2, 3]);  // 6

___

## Map and Set (ES6)

### Map

    var map = new Map();
    map.set('key', 'value');
    map.get('key');           // 'value'
    map.has('key');           // true
    map.delete('key');
    map.size;                 // number of entries
    map.clear();               // remove all
    
    // Iteration
    for (var [key, value] of map) {
      console.log(key, value);
    }

### Set

    var set = new Set([1, 2, 3]);
    set.add(4);
    set.has(3);               // true
    set.delete(2);
    set.size;                 // number of elements
    set.clear();              // remove all
    
    // Iteration
    for (var value of set) {
      console.log(value);
    }

___

## Symbols (ES6)

    var sym1 = Symbol();
    var sym2 = Symbol('description');
    var sym3 = Symbol.for('key');     // global symbol
    Symbol.keyFor(sym3);              // 'key'
    
    // Use case: unique object keys
    var obj = {};
    obj[Symbol('id')] = 1;
    obj[Symbol('id')] = 2;  // different symbols

___

## Generators (ES6)

    function* numberGenerator() {
      yield 1;
      yield 2;
      yield 3;
    }
    
    var gen = numberGenerator();
    gen.next();  // {value: 1, done: false}
    gen.next();  // {value: 2, done: false}
    gen.next();  // {value: 3, done: true}

___

## Proxy (ES6)

    var handler = {
      get: function(target, prop) {
        return prop in target ? target[prop] : 0;
      }
    };
    
    var p = new Proxy({}, handler);
    p.a = 1;
    p.b;  // 0 (default value)

___

## JSON

    JSON.stringify(obj);              // convert object to JSON string
    JSON.parse(jsonString);           // convert JSON string to object
    JSON.stringify(obj, null, 2);      // pretty print with 2-space indent

___

## Best Practices

1. **Use `const` by default, `let` when reassignment needed, avoid `var`**
2. **Use strict equality (`===` and `!==`) instead of loose equality**
3. **Use arrow functions for short callbacks**
4. **Use template literals for string interpolation**
5. **Use destructuring for cleaner code**
6. **Avoid `eval()` and `with` statement**
7. **Use `async/await` instead of promise chains when possible**
8. **Always handle errors with try/catch**
9. **Use meaningful variable and function names**
10. **Keep functions small and focused**
