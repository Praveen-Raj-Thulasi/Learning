# JavaScript Promises, Async & Await — Detailed Notes

---

# 1. Synchronous vs Asynchronous JavaScript

## Synchronous Code
JavaScript normally runs line by line.

```js
console.log("A");
console.log("B");
console.log("C");
```

### Output
```txt
A
B
C
```

Each line waits for the previous one to finish.

---

## Asynchronous Code
Some operations take time:

- API calls
- Database queries
- File reading
- Timers
- Fetch requests

JavaScript does not block execution for these tasks.

```js
console.log("Start");

setTimeout(() => {
    console.log("Inside timeout");
}, 2000);

console.log("End");
```

### Output
```txt
Start
End
Inside timeout
```

---

# 2. Why Asynchronous Programming?

If JavaScript waited for every API or database response synchronously:

- UI would freeze
- Browser becomes unresponsive
- Poor user experience

JavaScript solves this using:

- Event Loop
- Callback Queue
- Promises
- Async/Await

---

# 3. What is a Promise?

A Promise is an object representing a future result.

Think of it as:

> “I promise I’ll give you the result later.”

---

# 4. Promise States

A Promise has 3 states:

| State | Meaning |
|---|---|
| Pending | Still running |
| Fulfilled | Completed successfully |
| Rejected | Failed |

---

# 5. Creating a Promise

## Syntax

```js
const promise = new Promise((resolve, reject) => {

});
```

### Parameters

| Function | Purpose |
|---|---|
| resolve() | Success |
| reject() | Failure |

---

## Example

```js
const myPromise = new Promise((resolve, reject) => {
    let success = true;

    if(success) {
        resolve("Task completed");
    } else {
        reject("Task failed");
    }
});
```

---

# 6. Consuming a Promise

We use:

- `.then()`
- `.catch()`
- `.finally()`

## Example

```js
myPromise
    .then((result) => {
        console.log(result);
    })
    .catch((error) => {
        console.log(error);
    })
    .finally(() => {
        console.log("Finished");
    });
```

---

# 7. Promise Chaining

```js
fetchUser()
    .then(user => fetchPosts(user.id))
    .then(posts => console.log(posts))
    .catch(err => console.log(err));
```

Each `.then()` returns another promise.

---

# 8. Callback Hell

Before Promises:

```js
loginUser(username, password, () => {
    getProfile(() => {
        getPosts(() => {
            getComments(() => {
                console.log("Done");
            });
        });
    });
});
```

This becomes deeply nested and hard to manage.

Promises solve this problem.

---

# 9. Async and Await

Async/Await is syntactic sugar over promises.

## async

Makes a function return a promise.

```js
async function demo() {
    return "Hello";
}
```

---

## await

Pauses execution until a promise resolves.

```js
async function fetchData() {
    const response = await fetch(url);
    const data = await response.json();

    console.log(data);
}
```

---

# 10. Error Handling with try/catch

```js
async function getData() {
    try {
        const response = await fetch(url);
        const data = await response.json();

        console.log(data);
    } catch(error) {
        console.log("Error:", error);
    }
}
```

---

# 11. Promise.all()

Runs multiple promises in parallel.

```js
const p1 = Promise.resolve(10);
const p2 = Promise.resolve(20);

Promise.all([p1, p2])
    .then(values => console.log(values));
```

### Output

```txt
[10, 20]
```

---

# 12. Promise.race()

Returns the first completed promise.

```js
Promise.race([p1, p2])
    .then(result => console.log(result));
```

---

# 13. Event Loop

JavaScript is single-threaded.

The Event Loop handles:

- Call Stack
- Web APIs
- Callback Queue
- Microtask Queue

Promise callbacks go to the microtask queue.

`setTimeout()` callbacks go to the callback queue.

Microtasks execute before callbacks.

---

# 14. Important Interview Questions

1. Difference between async/await and promises?
2. What is callback hell?
3. What are promise states?
4. Difference between Promise.all and Promise.race?
5. What happens if await is used outside async?
6. Explain event loop and microtask queue.

---

# 15. Best Practices

- Always handle errors using catch or try/catch
- Avoid deeply nested promises
- Use async/await for readability
- Use Promise.all for parallel execution
- Avoid blocking operations
