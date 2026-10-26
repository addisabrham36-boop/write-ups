# HTTP in Detail

{% embed url="https://tryhackme.com/room/httpindetail" %}

A foundational room on the HyperText Transfer Protocol (HTTP), covering requests, responses, HTTP methods, status codes, headers, cookies, and interactive request building.

---

## 1. What is HTTP(S)?

- **HTTP:** HyperText Transfer Protocol.
- **HTTPS:** HyperText Transfer Protocol Secure. The **S** stands for **Secure**, indicating encryption via TLS/SSL.
- Looking at the mock browser in the task preview, clicking the broken certificate icon reveals the flag:
  `THM{INVALID_HTTP_CERT}`

---

## 2. Requests & Responses

An HTTP message consists of a request/status line, headers, and an optional body.
- Common protocol version in legacy/standard web: `HTTP/1.1`.
- The `Content-Length` header informs the client/server of the exact size of the payload body in bytes.

---

## 3. HTTP Methods

| Method | Purpose | Typical Scenario |
|---|---|---|
| **GET** | Retrieve data from a server | Viewing a blog post or webpage |
| **POST** | Submit data to be processed | Creating a user account or submitting a form |
| **PUT** | Replace/update an existing resource | Updating an email address |
| **DELETE** | Remove a resource | Deleting an uploaded picture |

---

## 4. Status Codes

- **201 Created:** Successful creation of a new resource (e.g. user account or blog post).
- **401 Unauthorized:** Authentication required or invalid credentials supplied.
- **404 Not Found:** Requested resource does not exist on the server.
- **503 Service Unavailable:** The server is temporarily unable to handle the request (e.g., database crash or overloaded).

---

## 5. Headers & Cookies

- `User-Agent`: Informs the server of the client's browser, OS, and rendering engine.
- `Content-Type`: Informs the recipient of the media type (MIME) of the resource (e.g., `text/html`, `application/json`).
- `Host`: Specifies the target domain/vhost being requested.
- `Set-Cookie`: Sent by the server to instruct the browser to store a session token or state cookie.

---

## 6. Practical Challenge

Using the interactive lab client to send a GET request to `/room`:

```http
GET /room HTTP/1.1
Host: website.thm
```

The response returns the practical flag:
`THM{YOU'RE_IN_THE_ROOM}`

---

## 🏁 Questions & Answers

| Task | Question | Answer |
|---|---|---|
| **Task 1** | What does HTTP stand for? | `HyperText Transfer Protocol` |
| **Task 1** | What does the S in HTTPS stand for? | `Secure` |
| **Task 1** | Flag from clicking the certificate issue | `THM{INVALID_HTTP_CERT}` |
| **Task 2** | What HTTP protocol version is used in the example? | `HTTP/1.1` |
| **Task 2** | Response header telling how much data to expect | `Content-Length` |
| **Task 3** | Method used to create a new user account | `POST` |
| **Task 3** | Method used to update your email address | `PUT` |
| **Task 3** | Method used to remove an uploaded picture | `DELETE` |
| **Task 3** | Method used to view a news article | `GET` |
| **Task 4** | Response code for created user or blog post | `201` |
| **Task 4** | Response code for a page that does not exist | `404` |
| **Task 4** | Response code when database crashes / server down | `503` |
| **Task 4** | Response code when editing profile without logging in | `401` |
| **Task 5** | Header telling the server what browser is used | `User-Agent` |
| **Task 5** | Header telling the browser what data type is returned | `Content-Type` |
| **Task 5** | Header telling the server which website is requested | `Host` |
| **Task 6** | Header used to save cookies to your computer | `Set-Cookie` |
| **Task 7** | Flag from GET request to `/room` | `THM{YOU'RE_IN_THE_ROOM}` |
