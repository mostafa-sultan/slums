# JavaScript ES6+ Features

## Introduction

ES6 (ECMAScript 2015) and subsequent versions introduced many powerful features that have become essential in modern JavaScript development. This tutorial covers the most important ES6+ features with practical examples.

---

## Let and Const

### let - Block Scoped Variables

    // Block scope
    if (true) {
      let x = 1;
      console.log(x); // 1
    }
    // console.log(x); // ReferenceError
    
    // No hoisting issues
    for (let i = 0; i < 3; i++) {
      setTimeout(() => console.log(i), 100); // 0, 1, 2
    }

### const - Constants

    const PI = 3.14159;
    // PI = 3.14; // TypeError
    
    // const with objects
    const person = { name: 'John' };
    person.name = 'Jane'; // OK
    person = {}; // TypeError

---

## Arrow Functions

### Basic Syntax

    // Traditional function
    function add(a, b) {
      return a + b;
    }
    
    // Arrow function
    const add = (a, b) => a + b;
    
    // Single parameter (no parentheses needed)
    const square = x => x * x;
    
    // No parameters
    const greet = () => 'Hello';
    
    // Multiple statements
    const process = (x) => {
      const doubled = x * 2;
      return doubled + 1;
    };

### this Binding

    // Traditional function (this changes)
    const obj = {
      name: 'John',
      traditional: function() {
        setTimeout(function() {
          console.log(this.name); // undefined
        }, 100);
      },
      arrow: function() {
        setTimeout(() => {
          console.log(this.name); // "John"
        }, 100);
      }
    };

---

## Template Literals

    const name = 'John';
    const age = 30;
    
    // String interpolation
    const message = `Hello, my name is ${name} and I am ${age} years old.`;
    
    // Multi-line strings
    const html = `
      <div>
        <h1>${name}</h1>
        <p>Age: ${age}</p>
      </div>
    `;
    
    // Expressions
    const calculation = `2 + 2 = ${2 + 2}`; // "2 + 2 = 4"
    
    // Tagged templates
    function highlight(strings, ...values) {
      return strings.reduce((result, str, i) => {
        return result + str + (values[i] ? `<mark>${values[i]}</mark>` : '');
      }, '');
    }
    
    const result = highlight`Hello ${name}, you are ${age} years old.`;

---

## Destructuring

### Array Destructuring

    const numbers = [1, 2, 3, 4, 5];
    
    // Basic
    const [first, second] = numbers;
    console.log(first, second); // 1, 2
    
    // Skip elements
    const [a, , c] = numbers; // 1, 3
    
    // Rest operator
    const [first, ...rest] = numbers; // 1, [2, 3, 4, 5]
    
    // Default values
    const [x = 10, y = 20] = [1]; // x = 1, y = 20
    
    // Swap variables
    let a = 1, b = 2;
    [a, b] = [b, a]; // a = 2, b = 1

### Object Destructuring

    const person = {
      name: 'John',
      age: 30,
      city: 'New York'
    };
    
    // Basic
    const { name, age } = person;
    
    // Rename variables
    const { name: personName, age: personAge } = person;
    
    // Default values
    const { name, email = 'no-email' } = person;
    
    // Rest operator
    const { name, ...rest } = person;
    
    // Nested destructuring
    const user = {
      id: 1,
      profile: {
        name: 'John',
        address: {
          city: 'New York'
        }
      }
    };
    
    const { profile: { name, address: { city } } } = user;

---

## Spread Operator

### Arrays

    const arr1 = [1, 2, 3];
    const arr2 = [4, 5, 6];
    
    // Combine arrays
    const combined = [...arr1, ...arr2]; // [1, 2, 3, 4, 5, 6]
    
    // Copy array
    const copy = [...arr1];
    
    // Add elements
    const withNew = [...arr1, 4, 5];
    
    // Function arguments
    function sum(a, b, c) {
      return a + b + c;
    }
    const numbers = [1, 2, 3];
    sum(...numbers); // 6

### Objects

    const obj1 = { a: 1, b: 2 };
    const obj2 = { c: 3, d: 4 };
    
    // Combine objects
    const combined = { ...obj1, ...obj2 }; // { a: 1, b: 2, c: 3, d: 4 }
    
    // Copy object
    const copy = { ...obj1 };
    
    // Override properties
    const updated = { ...obj1, b: 20 }; // { a: 1, b: 20 }

---

## Default Parameters

    function greet(name = 'Guest', greeting = 'Hello') {
      return `${greeting}, ${name}!`;
    }
    
    greet(); // "Hello, Guest!"
    greet('John'); // "Hello, John!"
    greet('John', 'Hi'); // "Hi, John!"
    
    // Expressions as defaults
    function createUser(name, id = Math.random().toString(36)) {
      return { name, id };
    }
    
    // Destructuring with defaults
    function processUser({ name = 'Anonymous', age = 0 } = {}) {
      return { name, age };
    }

---

## Rest Parameters

    function sum(...numbers) {
      return numbers.reduce((total, num) => total + num, 0);
    }
    
    sum(1, 2, 3, 4); // 10
    
    // Must be last parameter
    function greet(greeting, ...names) {
      return `${greeting}, ${names.join(', ')}!`;
    }
    
    greet('Hello', 'John', 'Jane', 'Bob'); // "Hello, John, Jane, Bob!"

---

## Classes

### Basic Class

    class Person {
      constructor(name, age) {
        this.name = name;
        this.age = age;
      }
      
      greet() {
        return `Hello, I'm ${this.name}`;
      }
      
      get info() {
        return `${this.name} is ${this.age} years old`;
      }
      
      set age(newAge) {
        if (newAge > 0) {
          this._age = newAge;
        }
      }
    }
    
    const person = new Person('John', 30);
    console.log(person.greet()); // "Hello, I'm John"

### Inheritance

    class Student extends Person {
      constructor(name, age, school) {
        super(name, age);
        this.school = school;
      }
      
      study() {
        return `${this.name} is studying at ${this.school}`;
      }
      
      greet() {
        return `${super.greet()} and I'm a student`;
      }
    }
    
    const student = new Student('Jane', 20, 'MIT');
    console.log(student.study());

### Static Methods

    class MathUtils {
      static add(a, b) {
        return a + b;
      }
      
      static PI = 3.14159;
    }
    
    MathUtils.add(1, 2); // 3
    console.log(MathUtils.PI); // 3.14159

---

## Modules

### Export

    // Named exports
    export const PI = 3.14159;
    export function add(a, b) {
      return a + b;
    }
    export class Calculator {
      // ...
    }
    
    // Default export
    export default class User {
      // ...
    }
    
    // Export list
    const name = 'John';
    const age = 30;
    export { name, age };
    
    // Rename exports
    export { name as userName, age as userAge };

### Import

    // Named imports
    import { PI, add } from './math.js';
    import { name, age } from './user.js';
    
    // Default import
    import User from './User.js';
    
    // Import all
    import * as math from './math.js';
    math.add(1, 2);
    
    // Rename imports
    import { name as userName } from './user.js';
    
    // Mixed
    import User, { name, age } from './user.js';

---

## Map and Set

### Map

    const map = new Map();
    
    // Set values
    map.set('name', 'John');
    map.set(1, 'one');
    map.set(true, 'yes');
    
    // Get values
    map.get('name'); // 'John'
    
    // Check existence
    map.has('name'); // true
    
    // Size
    map.size; // 3
    
    // Delete
    map.delete('name');
    
    // Iterate
    for (const [key, value] of map) {
      console.log(key, value);
    }
    
    // Convert from array
    const mapFromArray = new Map([
      ['key1', 'value1'],
      ['key2', 'value2']
    ]);

### Set

    const set = new Set([1, 2, 3, 3, 4]);
    console.log(set); // Set {1, 2, 3, 4}
    
    // Add
    set.add(5);
    
    // Check existence
    set.has(3); // true
    
    // Size
    set.size; // 5
    
    // Delete
    set.delete(3);
    
    // Iterate
    for (const value of set) {
      console.log(value);
    }
    
    // Convert to array
    const array = [...set];

---

## Symbols

    // Create symbol
    const sym1 = Symbol();
    const sym2 = Symbol('description');
    
    // Unique
    Symbol('id') === Symbol('id'); // false
    
    // Global symbols
    const globalSym = Symbol.for('key');
    Symbol.for('key') === globalSym; // true
    
    // Use as object keys
    const obj = {
      [Symbol('id')]: 1,
      name: 'John'
    };
    
    // Symbol properties are not enumerable
    Object.keys(obj); // ['name']
    Object.getOwnPropertySymbols(obj); // [Symbol(id)]

---

## Generators

    function* numberGenerator() {
      yield 1;
      yield 2;
      yield 3;
    }
    
    const gen = numberGenerator();
    console.log(gen.next()); // { value: 1, done: false }
    console.log(gen.next()); // { value: 2, done: false }
    console.log(gen.next()); // { value: 3, done: false }
    console.log(gen.next()); // { value: undefined, done: true }
    
    // Infinite generator
    function* infiniteNumbers() {
      let n = 0;
      while (true) {
        yield n++;
      }
    }
    
    // Delegation
    function* generator1() {
      yield 1;
      yield 2;
    }
    
    function* generator2() {
      yield* generator1();
      yield 3;
    }

---

## Proxy

    const target = {
      name: 'John',
      age: 30
    };
    
    const handler = {
      get: function(obj, prop) {
        return prop in obj ? obj[prop] : 'Not found';
      },
      set: function(obj, prop, value) {
        if (prop === 'age' && value < 0) {
          throw new Error('Age cannot be negative');
        }
        obj[prop] = value;
        return true;
      }
    };
    
    const proxy = new Proxy(target, handler);
    console.log(proxy.name); // 'John'
    console.log(proxy.unknown); // 'Not found'
    proxy.age = 25; // OK
    proxy.age = -5; // Error

---

## Optional Chaining (ES2020)

    const user = {
      name: 'John',
      address: {
        city: 'New York'
      }
    };
    
    // Safe property access
    const city = user?.address?.city; // 'New York'
    const zip = user?.address?.zip; // undefined (no error)
    
    // Safe method calls
    user?.getName?.(); // Only calls if exists
    
    // Safe array access
    const first = arr?.[0];
    
    // Combined with nullish coalescing
    const city = user?.address?.city ?? 'Unknown';

---

## Nullish Coalescing (ES2020)

    const value1 = null ?? 'default'; // 'default'
    const value2 = undefined ?? 'default'; // 'default'
    const value3 = 0 ?? 'default'; // 0 (not 'default')
    const value4 = '' ?? 'default'; // '' (not 'default')
    
    // Practical use
    const name = user.name ?? 'Anonymous';
    const count = items.length ?? 0;

---

## BigInt (ES2020)

    const bigNumber = 9007199254740991n;
    const another = BigInt('9007199254740991');
    
    // Operations
    bigNumber + 1n; // 9007199254740992n
    bigNumber * 2n; // 18014398509481982n
    
    // Cannot mix with regular numbers
    // bigNumber + 1; // TypeError

---

## Conclusion

ES6+ features have revolutionized JavaScript development, making code more readable, maintainable, and powerful. These features are now standard in modern JavaScript development and are essential for any JavaScript developer to master.

