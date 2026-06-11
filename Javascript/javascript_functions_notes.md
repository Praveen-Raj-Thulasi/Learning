# JavaScript Functions – Detailed Notes

# 1. Introduction to Functions

A function in JavaScript is a reusable block of code designed to perform a specific task.

Functions help:
- Reduce code repetition
- Improve readability
- Organize logic
- Make debugging easier

---

# 2. Function Declaration (Normal Functions)

## Syntax

```javascript
function functionName(parameters) {
    // code
}
```

## Example

```javascript
function greet(name) {
    console.log("Hello " + name);
}

greet("Praveen");
```

## Output

```javascript
Hello Praveen
```

## Explanation

- `function` → keyword used to declare a function
- `greet` → function name
- `name` → parameter
- `"Praveen"` → argument passed to function

---

# 3. Returning Values

Functions can return values using the `return` keyword.

## Example

```javascript
function add(a, b) {
    return a + b;
}

let result = add(5, 3);

console.log(result);
```

## Output

```javascript
8
```

---

# 4. Parameters vs Arguments

| Term | Meaning |
|------|----------|
| Parameter | Variable in function definition |
| Argument | Actual value passed |

## Example

```javascript
function multiply(x, y) {
    return x * y;
}

multiply(4, 5);
```

- `x` and `y` → parameters
- `4` and `5` → arguments

---

# 5. Function Expression

A function can also be stored inside a variable.

## Syntax

```javascript
const variableName = function(parameters) {
    // code
};
```

## Example

```javascript
const greet = function(name) {
    console.log("Hello " + name);
};

greet("Raj");
```

## Output

```javascript
Hello Raj
```

---

# 6. Difference Between Function Declaration and Function Expression

| Function Declaration | Function Expression |
|----------------------|--------------------|
| Hoisted completely | Not fully hoisted |
| Can call before declaration | Cannot call before initialization |
| Uses function keyword directly | Stored inside variable |

---

# 7. Hoisting

## Function Declaration Hoisting

```javascript
sayHello();

function sayHello() {
    console.log("Hello");
}
```

Works because declarations are hoisted.

---

## Function Expression Hoisting

```javascript
sayHi();

const sayHi = function() {
    console.log("Hi");
};
```

This gives an error.

Reason:
- Variable exists
- Function is not initialized yet

---

# 8. Arrow Functions

Introduced in ES6.

Provides shorter syntax.

## Syntax

```javascript
const functionName = (parameters) => {
    // code
};
```

## Example

```javascript
const add = (a, b) => {
    return a + b;
};

console.log(add(2, 3));
```

## Output

```javascript
5
```

---

# 9. Arrow Function Shortcuts

## Single Line Return

```javascript
const square = (n) => n * n;

console.log(square(4));
```

## Output

```javascript
16
```

---

## Single Parameter

```javascript
const greet = name => {
    console.log("Hello " + name);
};
```

Parentheses are optional for one parameter.

---

## No Parameters

```javascript
const hello = () => {
    console.log("Hello");
};
```

---

# 10. Traditional Function vs Arrow Function

| Traditional Function | Arrow Function |
|----------------------|----------------|
| Has its own `this` | Does not have own `this` |
| Uses `function` keyword | Uses `=>` |
| Suitable for object methods | Better for callbacks |

---

# 11. this Keyword Difference

## Traditional Function

```javascript
const person = {
    name: "Praveen",
    greet: function() {
        console.log(this.name);
    }
};

person.greet();
```

## Output

```javascript
Praveen
```

---

## Arrow Function

```javascript
const person = {
    name: "Praveen",
    greet: () => {
        console.log(this.name);
    }
};

person.greet();
```

## Output

```javascript
undefined
```

Arrow functions do not bind their own `this`.

---

# 12. Callback Functions

A function passed as argument to another function.

## Example

```javascript
function processUser(name, callback) {
    console.log("Processing " + name);
    callback();
}

function completed() {
    console.log("Done");
}

processUser("Praveen", completed);
```

## Output

```javascript
Processing Praveen
Done
```

---

# 13. Anonymous Functions

Functions without names.

## Example

```javascript
setTimeout(function() {
    console.log("Executed");
}, 2000);
```

---

# 14. Immediately Invoked Function Expression (IIFE)

Runs immediately after creation.

## Syntax

```javascript
(function() {
    console.log("IIFE executed");
})();
```

---

# 15. Default Parameters

## Example

```javascript
function greet(name = "Guest") {
    console.log("Hello " + name);
}

greet();
```

## Output

```javascript
Hello Guest
```

---

# 16. Rest Parameters

Allows multiple arguments.

## Example

```javascript
function sum(...numbers) {
    let total = 0;

    for(let num of numbers) {
        total += num;
    }

    return total;
}

console.log(sum(1,2,3,4));
```

## Output

```javascript
10
```

---

# 17. Higher Order Functions

Functions that:
- Accept functions as arguments
- Return functions

## Example

```javascript
function calculator(operation, a, b) {
    return operation(a, b);
}

function add(a, b) {
    return a + b;
}

console.log(calculator(add, 2, 3));
```

---

# 18. Pure Functions

A pure function:
- Gives same output for same input
- Has no side effects

## Example

```javascript
function add(a, b) {
    return a + b;
}
```

---

# 19. Recursive Functions

A function calling itself.

## Example

```javascript
function factorial(n) {
    if(n === 1)
        return 1;

    return n * factorial(n - 1);
}

console.log(factorial(5));
```

## Output

```javascript
120
```

---

# 20. Best Practices

- Use meaningful function names
- Keep functions short
- Avoid global variables
- Prefer arrow functions for callbacks
- Use function declarations for reusable utilities

---

# 21. Summary

| Type | Key Feature |
|------|--------------|
| Function Declaration | Fully hoisted |
| Function Expression | Stored in variable |
| Arrow Function | Short syntax |
| Callback Function | Passed as argument |
| Higher Order Function | Uses other functions |
| Recursive Function | Calls itself |

---

# 22. Quick Interview Questions

## Q1. Difference between function declaration and expression?

- Declaration is hoisted
- Expression is not fully hoisted

---

## Q2. Why are arrow functions used?

- Short syntax
- Cleaner callbacks
- Lexical `this`

---

## Q3. Can arrow functions be constructors?

No.

```javascript
const Person = (name) => {
    this.name = name;
};

new Person("Praveen");
```

This gives an error.

---

# End of Notes
