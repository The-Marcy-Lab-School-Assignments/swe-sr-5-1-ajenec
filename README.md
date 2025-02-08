# Technical Writing Assignment

For guidance on setting up and submitting this assignment, refer to the Marcy lab School Docs How-To guide for [Working with Short Response and Coding Assignments](https://marcylabschool.gitbook.io/marcy-lab-school-docs/fullstack-curriculum/how-tos/working-with-assignments#how-to-work-on-assignments).

## Prompt 1

Imagine you are teaching a friend about OOP. They mainly want to understand what is Encapsulation. Write a brief lesson on Encapsulation that includes the following:

- What is encapsulation?
- What major goal does this help to achieve in software engineering?
- Give an example (in code) of encapsulation.
- An explanation of how the code example demonstrates encapsulation

### Response 1

In **object-oriented programming (OOP)**, `encapsulation` is all about bundling variables and the methods that operate on the data into a single, easy-to-use unit. Think of it like a medicine capsule. The capsule has everything you need to feel better, but you can't see what's inside. All you know is the label on the outside, which tells you what it is. Just like with `encapsulation`, data, and methods are grouped together in a private unit. It conceals the inner workings and exposes only what is necessary.

The major goal that encapsulation helps achieve in software engineering is to hide and protect data from being directly accessed or modified outside of the object.

- **Data Hiding**: It ensures that the object's internal data is protected and can only be accessed or modified through defined methods.
- **Privacy**: It hides how the object is modified and allows the user of the object to focus only on what the object does.
- **Easy to Maintain**: Makes it easier to change the internal implementation without affecting the users of the object.
- **Control Over Data**: Allows the class to enforce rules about how data is accessed or modified.

```js
// This is the object
const makeBucketList = {
  toDo: [],

  // This function is the "method", which is stored inside the object
  addToList(newToDo) {
    // `this` refers to the "owner" of the method, which is the object it is invoked
    this.toDo.push(newToDo);
  },
  // This method allows you to see the data without directly accessing it
  printList() {
    console.log(this.toDo);
  },
};
```

**Data Hiding**: The `toDo` property is _encapsulated_ inside of the `makeBucketList` object. This property is not directly accessible from outside the object. You need to use the methods (`addToList` and `printList`).

**Access Control**: The `addToList` method is responsible for modifying the `toDo` array, and the `printList` method is used access and display the data. By controlling the access to the `toDo` array through these methods, it ensures that the data in the object cannot be modified outside the object easily. Which is the key of **encapsulation**.

## Prompt 2

The following `friendsManager` object is an example of an interface that is **NOT** consistent and predictable:

```js
const friendsManager = {
  friends: [],
  addFriend(newFriend) {
    if (typeof newFriend !== "string") return;
    this.friends.push(newFriend);
  },
};

friendsManager.addFriend("daniel");
friendsManager.addFriend(true);
friendsManager.friends.push("emmaneul");
friendsManager.friends.push(42);
```

Explain how the code is not consistent or predictable, then provide an example in code that uses closure to make it more consistent and predictable.

### Response 2

`friendsManager` function has some issues that prevents it from being consistent and predictable. The `friends` array is publicly accessible, meaning anyone can change it using `friendsManager.friends.push("friend")`. Since the array is publicly exposed, the user can bypass `addFriend()` method and push any data type (including non-string, boolean, etc.) values to the array which leads to inconsistent behavior of the output. We want to set up the `friends` array so that it can be modified only using the `addFriend()` method and not bypass any restrictions we specify in our method. Also `friendsManager` is a plain object and not a class-based or factory-based structure that allows to create multiple instances.

Here is an example of the revised code above that is consistent with OOP guidelines and is more predictable:

```js
class FriendsManager {
  // creates a private array where we can store all the friends of the individual instances
  #friends = [];

  addFriend(newFriend) {
    if (typeof newFriend !== "string") {
      console.log("Only strings allowed bro...");
      return;
    }
    this.#friends.push(newFriend);
  }

  getFriends() {
    // returns a shallow copy of the array to prevent the direct mutation
    return [...this.#friends];
  }
}

// creates a new instance
const danny = new FriendsManager();

// adds a new friend
danny.addFriends("Josh");

// does not add new friend because of the validation
danny.addFriends(213);

// returns ["Josh"]
console.log(danny.getFriends());

// throws a syntax error because cannot access the `friends` array directly
console.log(danny.#friends);
```

## Prompt 3

With OOP in JavaScript, it's possible to use factory functions to achieve encapsulation and re-use them to make objects that look alike. However, factory functions have drawbacks and we often use classes instead.

How would you explain to a budding developer what the drawbacks of using factory functions are and why it is better to use classes instead?

### Response 3

A `factorty function` is functions in JavaScript that can return objects. Here's an example:

```js
const favFoodsList = (...initialList) => {
  // Creates a new array 'list' that copies the passed initial foods
  const list = [...initialList];

  // Returns an object with two methods:
  return {
    // This method returns the current list
    getList() {
      return list;
    },

    // This method allows you to add to the list
    addToList(newFav) {
      list.push(newFav);
    },
  };
};

// Calling the factory function to create a new object
const myList = favFoodsList("french toast", "pancakes");

myList.addToList("fruit"); // Adds to current list: 'myList'
console.log(myList.getList()); // This logs: [ 'french toast', 'pancakes', 'fruit' ]
```

Every time the **factory function** is invoked, a brand new object is made with the same bahavior. Each object holds a seperate copy of the same methods. This increases memory usage because methods are stored independently for each object instead of sharing the method.

A `class` is a blueprint of something with variables and functions, used to create an object and execute its behavior. They are more structured and designed for managing objects.

```js
class GreetMe {
  greeting() {
    console.log("Hey Dude!");
  }
}

const buddy = new GreetMe();
buddy.greeting(); // Output: Hey Dude!
```

With Classes you can easily create subclasses that inherit properties and methods from the parent class. This allows you to reuse the code and keep things organized. When you create an object using a class, it uses the same set of methods making the code easier to maintain.

## Prompt 4

Do some research on the history of when / how classes were introduced into JavaScript and share your findings. Your response should include:

- What version of JavaScript were classes introduced in and when did it come out?
- Why were classes introduced into JavaScript?

### Response 4

- `Classes` were introduced in ES6 version of JavaScript and it came out in June 2015. Along with classes this version came out with a few more syntax features.

  1.  Class syntax (`class`, `constructor`, `extends`, `super`)
  2.  Arrow functions (`=>`)
  3.  `let` and `const` for block-scoped variables
  4.  Template literals (backticks `Hello ${name}`)
  5.  Default parameters
  6.  Destructuring assignment
  7.  Promises
  8.  Modules (`import`/`export`)

- `Classes` were introduced to JavaScript to offer a more structured and intuitive approach to object-oriented programming (OOP), making the syntax cleaner, more readable, and developer-friendly.

Here is a code example before `classes` were introduced (using prototype & constructor functions):

```js
function Person(name, age) {
  this.name = name;
  this.age = age;
}

Person.prototype.greet = function () {
  console.log(`Hello, my name is ${this.name}.`);
};
```

This syntax works but it is not intuitive for developers coming from class based languages.

Here is a revised example using `Classes` in ES6:

```js
class Person {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }

  greet() {
    console.log(`Hello, my name is ${this.name}.`);
  }
}
```

This is much cleaner syntax and resembles a traditional OOP languages.

`Classes` allow defining objects and dealing with inheritance much easier.
Here is an example of prototype-based inheritance before ES6:

```js
function Employee(name, age, jobTitle) {
  Person.call(this, name, age); // Call parent constructor
  this.jobTitle = jobTitle;
}

Employee.prototype = Object.create(Person.prototype);
Employee.prototype.constructor = Employee;

Employee.prototype.work = function () {
  console.log(`${this.name} is working as a ${this.jobTitle}.`);
};
```

Here is a better version using `Classes` with (`extends` & `super`)

```js
class Employee extends Person {
  constructor(name, age, jobTitle) {
    super(name, age); // Calls the parent class constructor
    this.jobTitle = jobTitle;
  }

  work() {
    console.log(`${this.name} is working as a ${this.jobTitle}.`);
  }
}
const emp1 = new Employee("Bob", 25, "Software Engineer");
emp1.greet(); // Inherited from Person class
emp1.work(); // Output: Bob is working as a Software Engineer.
```

This syntax is much easier to read and handle inheritance.

## Prompt 5

OOP can still be achieved in JavaScript without using the `class` keyword and instead using the "Constructor Functions" and the "Prototype Chain" (look them up!)

```js
function Person(name, age) {
  this.name = name;
  this.age = age;
}

Person.prototype.greet = function () {
  return `Hi, I'm ${this.name}, and I'm ${this.age} years old.`;
};

const alice = new Person("Alice", 30);
console.log(alice.greet());
```

Provide one point that advocates for the use of this syntax and then provide a counter-argument for the use of classes instead.

### Response 5

**Point for Constructure Functions and Prototype:**

- Using `Constructure Functions` and the `Prototype Chain` allows for more flexibility. You dont have to use the class keyword, which adds more structure. **Constructure Functions** allow you to create objects and define methods in a straightforward way. The **Prototype Chain** makes inheritance more transparent and customizable, giving you more control over how objects inherit behaviors.

**Point for Class:**

- `Classes` offer a cleaner, more modern, and more readable syntax for creating objects and managing inheritance. They are easier to understand for most developers, especially those coming from object-oriented programming (OOP) languages like Java or C++. Classes alse provide built-in support for inheritance using `extends`, simplifying the structure of your code.
