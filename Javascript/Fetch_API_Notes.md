# JavaScript Fetch API — Detailed Notes

---

# 1. What is Fetch API?

The Fetch API is a modern JavaScript interface used to make:

- API requests
- HTTP requests
- Network requests

It is used to communicate with servers.

---

# 2. Why Fetch API?

Before Fetch API, developers used:

- XMLHttpRequest (XHR)

Problems with XHR:

- Complex syntax
- Hard to read
- Callback hell

Fetch API provides:

- Cleaner syntax
- Promise-based handling
- Better readability
- Easier async programming

---

# 3. Basic Syntax

```js
fetch(url)
```

Fetch returns a Promise.

---

# 4. Simple GET Request

```js
fetch("https://jsonplaceholder.typicode.com/users")
    .then(response => response.json())
    .then(data => console.log(data))
    .catch(error => console.log(error));
```

---

# 5. Understanding the Flow

## Step 1

```js
fetch(url)
```

Sends request to server.

---

## Step 2

```js
.then(response => response.json())
```

Converts response into JSON.

---

## Step 3

```js
.then(data => console.log(data))
```

Uses the actual data.

---

## Step 4

```js
.catch(error => console.log(error))
```

Handles errors.

---

# 6. What Does fetch() Return?

Fetch returns a Promise.

```js
const result = fetch(url);

console.log(result);
```

Output:

```txt
Promise { <pending> }
```

---

# 7. Using Fetch with Async/Await

```js
async function getUsers() {
    try {
        const response = await fetch(
            "https://jsonplaceholder.typicode.com/users"
        );

        const data = await response.json();

        console.log(data);
    } catch(error) {
        console.log(error);
    }
}

getUsers();
```

---

# 8. HTTP Methods

| Method | Purpose |
|---|---|
| GET | Retrieve data |
| POST | Send new data |
| PUT | Update entire data |
| PATCH | Update partial data |
| DELETE | Remove data |

---

# 9. GET Request Example

```js
fetch("https://jsonplaceholder.typicode.com/posts")
    .then(response => response.json())
    .then(data => console.log(data));
```

---

# 10. POST Request Example

```js
fetch("https://jsonplaceholder.typicode.com/posts", {
    method: "POST",
    headers: {
        "Content-Type": "application/json"
    },
    body: JSON.stringify({
        title: "Hello",
        body: "Fetch API",
        userId: 1
    })
})
.then(response => response.json())
.then(data => console.log(data));
```

---

# 11. PUT Request Example

```js
fetch("https://jsonplaceholder.typicode.com/posts/1", {
    method: "PUT",
    headers: {
        "Content-Type": "application/json"
    },
    body: JSON.stringify({
        id: 1,
        title: "Updated Title",
        body: "Updated Body",
        userId: 1
    })
})
.then(response => response.json())
.then(data => console.log(data));
```

---

# 12. DELETE Request Example

```js
fetch("https://jsonplaceholder.typicode.com/posts/1", {
    method: "DELETE"
})
.then(response => console.log(response));
```

---

# 13. Headers in Fetch

Headers provide extra information to the server.

Example:

```js
headers: {
    "Content-Type": "application/json",
    "Authorization": "Bearer token"
}
```

---

# 14. response.json()

Converts JSON response into JavaScript object.

```js
const data = await response.json();
```

---

# 15. Checking Response Status

```js
fetch(url)
    .then(response => {
        if(!response.ok) {
            throw new Error("Request Failed");
        }

        return response.json();
    })
    .then(data => console.log(data))
    .catch(error => console.log(error));
```

---

# 16. Important Response Properties

| Property | Meaning |
|---|---|
| response.status | HTTP status code |
| response.ok | true if successful |
| response.headers | Response headers |
| response.url | Final URL |

---

# 17. Common HTTP Status Codes

| Code | Meaning |
|---|---|
| 200 | Success |
| 201 | Created |
| 400 | Bad Request |
| 401 | Unauthorized |
| 404 | Not Found |
| 500 | Server Error |

---

# 18. Handling Errors Properly

```js
async function fetchData() {
    try {
        const response = await fetch(url);

        if(!response.ok) {
            throw new Error("HTTP Error");
        }

        const data = await response.json();

        console.log(data);
    } catch(error) {
        console.log(error.message);
    }
}
```

---

# 19. Fetch API vs Axios

| Feature | Fetch | Axios |
|---|---|---|
| Built into browser | Yes | No |
| Automatic JSON parsing | No | Yes |
| Uses Promises | Yes | Yes |
| Request cancellation | Limited | Better |

---

# 20. Sending Query Parameters

```js
fetch("https://api.com/users?id=1")
```

Example:

```js
fetch(`https://api.com/search?q=javascript`)
```

---

# 21. Fetch API with Headers and Token

```js
fetch(url, {
    headers: {
        Authorization: "Bearer myToken"
    }
})
```

---

# 22. Common Mistakes

## Forgetting await

Wrong:

```js
const data = response.json();
```

Correct:

```js
const data = await response.json();
```

---

## Forgetting JSON.stringify()

Wrong:

```js
body: {
    name: "John"
}
```

Correct:

```js
body: JSON.stringify({
    name: "John"
})
```

---

# 23. Real-World Example

```js
async function getMeals() {
    try {
        const response = await fetch(
            "https://www.themealdb.com/api/json/v1/1/search.php?s=chicken"
        );

        const data = await response.json();

        console.log(data.meals);
    } catch(error) {
        console.log(error);
    }
}

getMeals();
```

---

# 24. Best Practices

- Always use try/catch
- Check response.ok
- Use async/await for readability
- Keep API URLs in constants
- Avoid deeply nested .then()

---

# 25. Important Interview Questions

1. What is Fetch API?
2. Difference between Fetch and Axios?
3. Why does fetch return a Promise?
4. What does response.json() do?
5. Difference between PUT and PATCH?
6. How do you handle errors in fetch?
7. What is response.ok?
8. How do you send headers in fetch?

---

# 26. Summary

Fetch API is:

- Modern
- Promise-based
- Cleaner than XMLHttpRequest
- Used for HTTP communication
- Common in frontend development

Fetch works extremely well with:

- Promises
- Async/Await
- REST APIs
