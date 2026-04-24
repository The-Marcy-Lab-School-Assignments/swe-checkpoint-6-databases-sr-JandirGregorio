# Short Response: Databases Checkpoint

Answer each question below in complete sentences. Aim for 3–5 sentences per answer — enough to show that you understand the concept, not just that you memorized a definition. Use the exact terms and concepts from the lessons, but write in your own words. Specific examples and analogies are encouraged.

---

## Question 1

What is the difference between **authentication** and **authorization**? Give a concrete example of how a user would encounter each in the context of a fullstack web application.

**Your answer:** **Authentication** refers to how the application verifies a user's identity, essentially asking, _"who are you?"_ **Authorization** refers to what a user is allowed to do once they are logged into the application. In a full stack application,  authentication happens when a user logs in and the application verifies their credentials. After a user is authenticated, authorization determines which actions they have permission to perform within the app. For example, a user may be logged in and authorized to edit their own account, but not someone else's.


---

## Question 2

Why should passwords **never** be stored as plaintext in a database? Explain what hashing is and its key properties that allow a server to verify a password without ever storing the original?

**Your answer:** Passwords should never be stored as plaintext in a database because, in the case of a data breach, attackers could use them to access not only the current account but the user’s other accounts, since people often reuse passwords across sites. Hashing is a technique that converts plaintext into non-human readable strings.

For hashing to work properly, it must be **one-way**, meaning a password can be hashed but not reversed back into its original form, and **deterministic**, meaning the same input always produces the same hash. This is what makes password verification possible. However, hashing alone is not enough; it also needs **salting**, which adds a random value before hashing based on **salt rounds**. This makes the hash more secure and helps prevent attackers from using rainbow tables to look up commonly used passwords.

---

## Question 3

Explain what it means when we say that "HTTP is stateless"? Explain why cookies are necessary in order to keep users logged-in across multiple sessions and how a server and a client work together to achieve this functionality.

**Your answer:** HTTP is called stateless because it treats every incoming request as independent and does not remember previous requests on its own. Cookies are necessary because they allow the browser to store a small piece of data that helps identify the user across requests. This makes it possible for users to stay logged in instead of having to authenticate again on every page load.

For example, when a user logs in, the server can create a session and store the user’s ID on `req.session.userId`, while the browser stores the session cookie. On future requests, the browser sends that cookie to the server, and the server uses it to look up the session and confirm the user’s identity.

---

## Question 4

A frontend can hide a "Delete Account" button from users who aren't logged in. Why isn't that enough to protect the `DELETE /api/users/:id` route on the server? What are the two layers of protection that the backend implements to protect against this?

**Your answer:** The frontend layer is not enough because attackers can bypass the interface entirely by sending requests with tools like `curl` or Postman. If the server does not protect the route, someone could try to delete a user account just by knowing or guessing a user ID.

The backend protects against this with two layers:

1. Authentication, which checks whether the client is logged in with a valid session or cookie, and
2. Authorization, which checks to perform that specific action.

Without authorization, an authenticated user could still send a request to delete someone else's account.

---

## Question 5

What is **SQL injection**? Explain what makes the code below unsafe, then describe how parameterized queries fix the problem.

```js
// Unsafe — never do this!
pool.query(`SELECT * FROM users WHERE username = '${username}'`);
```

**Your answer:** **SQL injection** is when a malicious user inserts SQL code into an input field in order to change the intended behavior of a query. The code above is unsafe because it uses string interpolation to place user input directly into the SQL statement. That means a user could enter SQL statements that change the query or even perform destructive actions, such as exposing data or deleting tables.

Parameterized queries fix this problem by separating the SQL structure from the user-provided values. Instead of inserting the value directly into the string, they use placeholders such as `$1`, which prevents the input from being interpreted as SQL code.

---

## Question 6

What problem does the **`/api/auth/me`** Endpoint pattern solve? When does the frontend call it and what does it return?

**Your answer:** The `api/auth/me` endpoint solves the problem of the frontend not knowing who the currently logged-in user is after a page refresh. Even if the session cookie still exists, the frontend needs a way to get the user’s information again so it can render the correct view. The frontend calls this endpoint when the application first loads or refreshes. If the user is authenticated, the endpoint returns information about the current user, such as their ID, username, or other user data. This allows the application to keep the user logged in and display the correct interface.

---
