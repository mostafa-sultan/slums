# JavaScript Prototypes and Inheritance

## Introduction

JavaScript uses prototypes for inheritance, which is different from class-based inheritance in other languages. Understanding prototypes is essential for mastering JavaScript. This tutorial covers prototypes, prototype chains, and how to use them effectively.

---

## What are Prototypes?

Every JavaScript object has a prototype. A prototype is an object from which other objects inherit properties and methods. When you access a property on an object, JavaScript first looks for it on the object itself, then on its prototype, then on the prototype's prototype, and so on.

### The Prototype Chain

    const obj = {};
    console.log(obj.toString); // function (from Object.prototype)
    
    // obj -> Object.prototype -> null

---

## Understanding __proto__ and prototype

### __proto__

`__proto__` is a property that points to the object's prototype:

    const obj = {};
    console.log(obj.__proto__ === Object.prototype); // true

### prototype Property

Functions have a `prototype` property that is used when the function is used as a constructor:

    function Person(name) {
      this.name = name;
    }
    
    Person.prototype.greet = function() {
      return `Hello, I'm ${this.name}`;
    };
    
    const person = new Person('John');
    console.log(person.greet()); // "Hello, I'm John"
    console.log(person.__proto__ === Person.prototype); // true

---

## Creating Objects with Prototypes

### Object.create()

Creates a new object with the specified prototype:

    const animal = {
      makeSound: function() {
        return 'Some sound';
      }
    };
    
    const dog = Object.create(animal);
    dog.breed = 'Labrador';
    dog.makeSound = function() {
      return 'Woof!';
    };
    
    console.log(dog.makeSound()); // "Woof!"
    console.log(dog.__proto__ === animal); // true

### Constructor Functions

    function Animal(name) {
      this.name = name;
    }
    
    Animal.prototype.makeSound = function() {
      return 'Some sound';
    };
    
    function Dog(name, breed) {
      Animal.call(this, name);
      this.breed = breed;
    }
    
    // Set up inheritance
    Dog.prototype = Object.create(Animal.prototype);
    Dog.prototype.constructor = Dog;
    
    Dog.prototype.makeSound = function() {
      return 'Woof!';
    };
    
    const dog = new Dog('Buddy', 'Labrador');
    console.log(dog.name); // "Buddy"
    console.log(dog.makeSound()); // "Woof!"

---

## ES6 Classes and Prototypes

ES6 classes are syntactic sugar over prototypes:

    class Animal {
      constructor(name) {
        this.name = name;
      }
      
      makeSound() {
        return 'Some sound';
      }
    }
    
    class Dog extends Animal {
      constructor(name, breed) {
        super(name);
        this.breed = breed;
      }
      
      makeSound() {
        return 'Woof!';
      }
    }
    
    const dog = new Dog('Buddy', 'Labrador');
    console.log(dog.makeSound()); // "Woof!"
    console.log(dog instanceof Animal); // true

This is equivalent to the prototype-based code above.

---

## Prototype Chain Examples

### Simple Chain

    const obj = {
      a: 1
    };
    
    // obj -> Object.prototype -> null
    console.log(obj.toString); // function (from Object.prototype)
    console.log(obj.hasOwnProperty('a')); // true
    console.log(obj.hasOwnProperty('toString')); // false

### Inheritance Chain

    function Grandparent() {}
    Grandparent.prototype.grandparentMethod = function() {
      return 'Grandparent';
    };
    
    function Parent() {}
    Parent.prototype = Object.create(Grandparent.prototype);
    Parent.prototype.constructor = Parent;
    Parent.prototype.parentMethod = function() {
      return 'Parent';
    };
    
    function Child() {}
    Child.prototype = Object.create(Parent.prototype);
    Child.prototype.constructor = Child;
    Child.prototype.childMethod = function() {
      return 'Child';
    };
    
    const child = new Child();
    console.log(child.childMethod()); // "Child"
    console.log(child.parentMethod()); // "Parent"
    console.log(child.grandparentMethod()); // "Grandparent"

---

## Checking Prototypes

### instanceof

Checks if an object is an instance of a constructor:

    function Person() {}
    const person = new Person();
    
    console.log(person instanceof Person); // true
    console.log(person instanceof Object); // true

### isPrototypeOf()

Checks if an object exists in another object's prototype chain:

    const animal = { name: 'Animal' };
    const dog = Object.create(animal);
    
    console.log(animal.isPrototypeOf(dog)); // true
    console.log(Object.prototype.isPrototypeOf(dog)); // true

### hasOwnProperty()

Checks if a property exists on the object itself (not inherited):

    const obj = {
      ownProp: 'value'
    };
    
    console.log(obj.hasOwnProperty('ownProp')); // true
    console.log(obj.hasOwnProperty('toString')); // false

### Object.getPrototypeOf()

Gets the prototype of an object:

    const obj = {};
    console.log(Object.getPrototypeOf(obj) === Object.prototype); // true

---

## Modifying Prototypes

### Adding Methods to Built-in Objects

    // Add method to Array prototype
    Array.prototype.last = function() {
      return this[this.length - 1];
    };
    
    const arr = [1, 2, 3];
    console.log(arr.last()); // 3
    
    // Warning: Modifying built-in prototypes is generally not recommended

### Extending Prototypes Safely

    // Check if method exists before adding
    if (!Array.prototype.last) {
      Array.prototype.last = function() {
        return this[this.length - 1];
      };
    }

---

## Prototype Patterns

### Prototype Pattern

    function Person(name) {
      this.name = name;
    }
    
    Person.prototype = {
      constructor: Person,
      greet: function() {
        return `Hello, I'm ${this.name}`;
      },
      introduce: function() {
        return `My name is ${this.name}`;
      }
    };
    
    const person = new Person('John');
    console.log(person.greet()); // "Hello, I'm John"

### Mixins

    const canFly = {
      fly: function() {
        return 'Flying!';
      }
    };
    
    const canSwim = {
      swim: function() {
        return 'Swimming!';
      }
    };
    
    function Duck() {}
    Object.assign(Duck.prototype, canFly, canSwim);
    
    const duck = new Duck();
    console.log(duck.fly()); // "Flying!"
    console.log(duck.swim()); // "Swimming!"

---

## Performance Considerations

### Prototype vs Instance Properties

    function Person(name) {
      this.name = name; // Instance property
    }
    
    Person.prototype.greet = function() {
      // Prototype method (shared)
      return `Hello, I'm ${this.name}`;
    };
    
    // Instance properties: one per object
    // Prototype methods: shared across all instances (more memory efficient)

### Property Lookup Performance

JavaScript engines optimize prototype lookups, but deep prototype chains can be slower:

    // Shallow chain: faster
    obj -> Object.prototype -> null
    
    // Deep chain: slower
    obj -> Level1 -> Level2 -> Level3 -> Object.prototype -> null

---

## Common Patterns

### Constructor with Prototype Methods

    function Calculator() {
      this.result = 0;
    }
    
    Calculator.prototype.add = function(x) {
      this.result += x;
      return this;
    };
    
    Calculator.prototype.subtract = function(x) {
      this.result -= x;
      return this;
    };
    
    Calculator.prototype.getValue = function() {
      return this.result;
    };
    
    const calc = new Calculator();
    calc.add(10).subtract(5);
    console.log(calc.getValue()); // 5

### Factory with Prototype

    function createPerson(name) {
      const person = Object.create(personPrototype);
      person.name = name;
      return person;
    }
    
    const personPrototype = {
      greet: function() {
        return `Hello, I'm ${this.name}`;
      }
    };
    
    const person = createPerson('John');
    console.log(person.greet()); // "Hello, I'm John"

---

## Best Practices

1. **Use ES6 classes** for cleaner syntax (they use prototypes under the hood)
2. **Don't modify built-in prototypes** unless absolutely necessary
3. **Use Object.create()** for prototype-based inheritance
4. **Set constructor property** when replacing prototype
5. **Use hasOwnProperty()** to check for own properties
6. **Prefer composition over deep inheritance**
7. **Understand the prototype chain** for debugging

---

## Common Mistakes

### Mistake 1: Forgetting constructor

    function Parent() {}
    function Child() {}
    
    Child.prototype = Object.create(Parent.prototype);
    // Missing: Child.prototype.constructor = Child;

### Mistake 2: Modifying prototype after instantiation

    function Person() {}
    const person = new Person();
    Person.prototype.newMethod = function() {};
    // person.newMethod() works, but can be confusing

### Mistake 3: Replacing prototype incorrectly

    function Person() {}
    Person.prototype = {
      // Missing constructor property
      greet: function() {}
    };

---

## Conclusion

Prototypes are fundamental to JavaScript. Whether you use constructor functions, ES6 classes, or Object.create(), understanding how prototypes work will help you write better JavaScript code and debug issues more effectively.

