# Short Response: Databases Checkpoint

Answer each question below in complete sentences. Aim for 3–5 sentences per answer — enough to show that you understand the concept, not just that you memorized a definition. Use the exact terms and concepts from the lessons, but write in your own words. Specific examples and analogies are encouraged.

---

## Question 1

What is the difference between **authentication** and **authorization**? Give a concrete example of how a user would encounter each in the context of a fullstack web application.

**Your answer:** **Authentication** refers to how the application verifies the user's identity, essentially asking _"who are you?"._ **Authorization** is what a user is allowed to do when logged in in the application. In a fullstack application,  authentication shows when a user is logging in and the application verifies their credentials. After a user is authenticated, then it's a matter of permission to do certain tasks within the app. For instance, a user can be logged in but it's authorized to edit their own account, not someone else's.

---

## Question 2

Why should passwords **never** be stored as plaintext in a database? Explain what hashing is and its key properties that allow a server to verify a password without ever storing the original?

**Your answer:** Passwords should never be stored as plaintext in a database because in the case of a data breach, attrackers can use them to access not only the current account, but also their other accounts, as people tend to reuse them across sites. Hashing is a programming technique that converts plaintext into non-human readable strings.

Hashing needs two things to work properly:
1. It needs to be **one-way**, meaning you can encript a password but cannot reverse it.
2. It has to be deterministic, meaning the same input will produce the same password encryption. This is what makes password verifyfication possible.

However, this is not enough and it also needs **salting**: a random string added to the hashing that produces a new hash based on **salt rounds**. This enhances the security and prevent attackers from using a rainbow table to look up commonly used passwords.

---

## Question 3

Explain what it means when we say that "HTTP is stateless"? Explain why cookies are necessary in order to keep users logged-in across multiple sessions and how a server and a client work together to achieve this functionality.

**Your answer:** An HTTP is said to be stateless because it treats every incoming request as a new one. It doesn't "remember" previously requests made. Cookies useful, as it stores a user's piece of data - usually their id number - to verify who they are at all times. This makes it possible for users to stay logged in. 

Let's suppose that the client is the user's browser. The browser will send a request to the server when the user logs in or registers a new account. Then the server will use the user's id to create a `userId` property to add to the `session` object. After this is done, the server will verify the user's id against the stored id in `userId` to verify their identify and keeps them logged in.

---

## Question 4

A frontend can hide a "Delete Account" button from users who aren't logged in. Why isn't that enough to protect the `DELETE /api/users/:id` route on the server? What are the two layers of protection that the backend implements to protect against this?

**Your answer:** The frontend layer is not enough because attackers using `curl` commands can surpass this barrier and can delete users if they know their `id`. The backend prevents this attacks by adding:

1. an authentication layer that verifies if the client has a valid cookie stored of the user's `id` and
2. an authorization layer that only allows the logged in user to verify their own account and not others.

Without the authorization layer, authenticated users could still send requests to the server to and delete someone else's account.

---

## Question 5

What is **SQL injection**? Explain what makes the code below unsafe, then describe how parameterized queries fix the problem.

```js
// Unsafe — never do this!
pool.query(`SELECT * FROM users WHERE username = '${username}'`);
```

**Your answer:** **SQL injection** are malicious queries inserted by the user in a form to break the intended logic of an application. The code above is unsafe because it uses string interpolation, embedding the user's input directly into the query. This would mean that if a user writes a malicious query to delete a table, they can succesfully achieve it.

Parameterized queries prevent this behavior by handling the values separately from the SQL structure. It uses placeholders to reference the values in the format of `$NUMBER`.

---

## Question 6

What problem does the **`/api/auth/me`** endpoint pattern solve? When does the frontend call it and what does it return?

**Your answer:** With just the cookie, we are able to know who just logged in/registered into the application and present them with their respective views. However, if they refresh the page, they're logged out of their accounts. The `/api/auth/me` endpoint solves this so users can stay logged in in an application. The frontend calls this endpoint on every page load-up and retrieves the user's id to verify the user's identify to render view based accordingly.

---
