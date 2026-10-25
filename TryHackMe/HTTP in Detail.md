# HTTP in Detail: TryHackMe Walkthrough

Official room: https://tryhackme.com/room/httpindetail

## Room Overview & Information

**Category:** Web fundamentals  
**Difficulty:** Beginner  
**Focus:** HTTP and HTTPS, URLs, request/response structure, methods, status codes, headers, cookies, and constructing requests.

HTTP is the application protocol browsers and web servers use to exchange resources. Understanding the request and response format makes web debugging and security testing much less mysterious. HTTPS is HTTP protected by TLS; TLS provides transport encryption and helps authenticate the server through certificates.

This room is a protocol-learning lab, not a boot-to-root target. There is no Nmap exploitation chain or privilege escalation phase. The useful evidence comes from the interactive HTTP emulator and the protocol examples shown in the task.

## Phase 1: Understand URLs and requests

A URL can contain a scheme, user information, host, port, path, query string, and fragment. Not every URL uses every component.

Example:

```text
https://example.test:8443/articles?id=7#comments
```

- `https` is the scheme.
- `example.test` is the host.
- `8443` is the explicit port.
- `/articles` is the path.
- `id=7` is a query parameter.
- `comments` is a fragment handled by the client; it is not normally sent to the server in the HTTP request.

A request line contains a method, path, and HTTP version. The Host header identifies the requested hostname, which matters when one IP hosts multiple websites. Headers carry metadata, and a blank line separates the headers from an optional body.

A simple request can be made from a terminal with:

```bash
curl -i https://example.com/
```

- `curl` makes the HTTP request.
- `-i` includes response headers in the output.
- The URL identifies the scheme and destination.

This command is an illustrative example, not a command required to complete the room's browser emulator.

## Phase 2: HTTP methods

Methods describe the intended operation. They do not, by themselves, enforce authorisation.

| Method | Common purpose | Room-style example |
|---|---|---|
| `GET` | Retrieve a representation | View a news article |
| `POST` | Submit data, often creating a resource or starting an operation | Create a user account or submit a login form |
| `PUT` | Create or replace the state of a resource at a known URI | Update a user resource |
| `DELETE` | Request removal of a resource | Remove an uploaded picture |

A real application may use methods differently, and a server may reject a method that the application does not support. The room's questions use the conventional mappings above.

### Task 3: HTTP Methods, question-by-question

**Question: “What method would be used to create a new user account?”**

Use `POST`. Account creation usually submits new data to the server.

**Question: “What method would be used to update your email address?”**

Use `PUT` in the room's simplified method mapping.

**Question: “What method would be used to remove a picture you've uploaded to your account?”**

Use `DELETE`.

**Question: “What method would be used to view a news article?”**

Use `GET`.

The lesson is to identify the intended action, then match it to the conventional method. In a real application, verify the documented API rather than assuming that every endpoint follows these conventions.

## Phase 3: HTTP status codes

The first line of an HTTP response includes the status code. The first digit indicates a broad category:

- **1xx:** informational; the request process is continuing.
- **2xx:** success.
- **3xx:** redirection or another action needed to complete the request.
- **4xx:** client-side request or authorisation problem.
- **5xx:** server-side failure.

Common examples:

| Code | Meaning | Practical interpretation |
|---|---|---|
| `200 OK` | Request succeeded | The response contains the requested result |
| `201 Created` | Resource created | Common after successful creation |
| `301 Moved Permanently` | Permanent redirect | Client should use the new URI |
| `302 Found` | Temporary redirect | Client is redirected elsewhere |
| `400 Bad Request` | Request is malformed or invalid | Inspect syntax and required fields |
| `401 Unauthorized` | Authentication is required or failed | Provide valid authentication |
| `403 Forbidden` | Server refuses the action | Identity may be known but access is not permitted |
| `404 Not Found` | Resource not found or intentionally hidden | Check path and application behaviour |
| `405 Method Not Allowed` | Method unsupported for this resource | Use the method allowed by the endpoint |
| `500 Internal Server Error` | Unexpected server failure | Server logs are needed to identify the cause |
| `503 Service Unavailable` | Service temporarily unavailable | May indicate maintenance or overload |

A status code is a signal, not a full diagnosis. A 404 can be deliberately used to conceal a protected resource, and a 200 response can still contain an application-level error.

## Phase 4: Headers

Headers provide metadata about a request or response.

### Request headers

- `Host`: hostname requested by the client.
- `User-Agent`: client software identifier.
- `Content-Length`: length of the request body in bytes.
- `Accept-Encoding`: compression formats the client can accept.
- `Cookie`: cookies sent back to the server.

### Response headers

- `Set-Cookie`: instructs the browser to store a cookie.
- `Content-Type`: identifies the type of content being returned.
- `Content-Encoding`: indicates response compression.
- `Cache-Control`: controls caching behaviour.

### Task 5: Headers, question-by-question

**Question: “What header tells the web server what browser is being used?”**

The conventional answer is `User-Agent`.

**Question: “What header tells the browser what type of data is being returned?”**

The answer is `Content-Type`.

**Question: “What header tells the web server which website is being requested?”**

The answer is `Host`.

These headers can be inspected in a browser's developer tools under the Network tab or with a command such as `curl -i`. User-Agent is client-supplied and can be changed, so it is not a trustworthy proof of a user's actual browser or identity.

## Phase 5: Cookies and sessions

HTTP is stateless: a server does not automatically remember a prior request. Cookies let a server and browser associate later requests with a session or preference.

A server sends a cookie using `Set-Cookie`; the browser can then send it back in a `Cookie` request header. A session cookie may act as a bearer credential, so anyone who steals a valid session token may be able to impersonate its owner.

### Task 6: Cookies, question-by-question

**Question: “Which header is used to save cookies to your computer?”**

The answer is `Set-Cookie`.

To inspect cookies in a browser, open Developer Tools, select **Network**, reload the lab page, choose a request, and inspect its request and response headers or Cookies tab. The response's `Set-Cookie` header shows what the server asks the browser to store; the later request's `Cookie` header shows what the browser sends.

Secure cookie configuration commonly includes:
- `Secure`, so the cookie is sent only over HTTPS.
- `HttpOnly`, so ordinary page JavaScript cannot read it.
- `SameSite`, to control when the browser sends it in cross-site contexts.
- A suitable `Path`, `Domain`, and lifetime.

These attributes reduce risk but do not replace correct server-side authorisation and session management.

## Phase 6: Interactive request emulator

The room's final task uses a website emulator. Click **View Site** in the task and build each request using the method selector, URI path, query parameters, and body parameters. Do not confuse query parameters with body parameters.

### Task 7: Making Requests

**Question: “Make a GET request to /room page”**

1. Select `GET`.
2. Set the path to `/room`.
3. Send the request.
4. Confirm the emulator shows the request and response.

**Question: “Make a GET request to /blog page and set the id parameter to 1”**

1. Select `GET`.
2. Set the path to `/blog`.
3. Open the URI-parameter control (the gear button).
4. Add parameter `id` with value `1`.
5. Send the request.

The request is conceptually `GET /blog?id=1`.

**Question: “Make a DELETE request to /user/1 page”**

1. Select `DELETE`.
2. Set the path to `/user/1`.
3. Send the request.

**Question: “Make a PUT request to /user/2 page with the username parameter set to admin”**

1. Select `PUT`.
2. Set the path to `/user/2`.
3. Open the body-parameter control.
4. Add `username=admin`.
5. Send the request.

**Question: “Make a POST request to /login page with the username of thm and a password of letmein”**

1. Select `POST`.
2. Set the path to `/login`.
3. Open the body-parameter control.
4. Add `username=thm`.
5. Add `password=letmein`.
6. Send the request.

These values are part of the room's local emulator. Do not reuse the example credentials on real services.

## Phase 7: Troubleshooting and verification

If the emulator rejects a request, check the method first, then the path, then whether each value belongs in the query string or body. A query parameter appears after `?` in the URI; body parameters are sent in the request body. In a real application, the server may also require cookies, CSRF tokens, or authentication.

Useful local inspection command:

```bash
curl -i -X GET 'https://example.com/'
```

Here, `-X GET` explicitly sets the method, although GET is already curl's default for a plain URL. Use this as a learning example, not as a replacement for the task emulator.

## Phase 8: Key takeaways and mitigation

- Use HTTPS for confidentiality and server authentication in transit.
- Validate input on the server and enforce authorisation for every action.
- Do not treat HTTP methods as security controls.
- Set cookies with appropriate security attributes.
- Avoid exposing detailed internal errors to unauthenticated users.
- Log relevant request metadata without unnecessarily recording secrets.
- Use correct cache controls for sensitive responses.

The main lesson is that HTTP is structured communication. Once I can read the method, path, headers, body, and status code separately, I can understand what a browser is asking for and what the server is returning.

## Expanded HTTP practical reference

### 1. Inspect a request and response

HTTP is an application protocol used to exchange requests and responses. A request contains a method, path, version, headers, and sometimes a body. A response contains a status code, headers, and optional content.

Use curl to inspect a known lab URL:

```bash
curl -i http://TARGET_HOST/
curl -I http://TARGET_HOST/
```

`-i` prints response headers with the body; `-I` sends a HEAD request and displays headers only. A server may handle HEAD differently from GET, so use `-i` when the task needs the actual page response.

### 2. Methods and status codes

GET retrieves a representation; POST submits data; PUT commonly replaces a resource; PATCH applies a partial update; DELETE requests removal. Actual authorization and side effects depend on the application. A method name alone does not prove the action is permitted.

Status codes are grouped into informational (1xx), success (2xx), redirection (3xx), client error (4xx), and server error (5xx). Common examples include 200 OK, 301/302 redirects, 400 malformed request, 401 authentication required or failed, 403 forbidden, 404 not found, 429 rate limited, and 500 internal server error. Interpret them in context; applications sometimes use codes inconsistently.

### 3. Headers, cookies, and sessions

Headers carry metadata such as Host, User-Agent, Content-Type, Authorization, and caching controls. Cookies may contain a session identifier or preferences. Treat session cookies and bearer tokens as secrets; redact them from screenshots and public notes.

Inspect redirect behaviour and cookies:

```bash
curl -i -L http://TARGET_HOST/
```

`-L` follows redirects. Use it only when the task needs the final destination; otherwise the first response may contain the relevant Location header.

### 4. Content types and request bodies

Content-Type describes the format of a request or response body, such as application/json or application/x-www-form-urlencoded. A mismatch between the declared type and actual content can cause validation errors or expose parsing inconsistencies. Inspect the request in browser Developer Tools or an intercepting proxy when the room asks about form submissions.

### 5. HTTPS and TLS

HTTPS wraps HTTP in TLS to provide confidentiality and integrity in transit and to authenticate the server according to certificate validation. A padlock does not prove the website's business is legitimate or that its application is secure. Certificate warnings should not be ignored on real services.

### 6. Troubleshooting and task answers

If a request fails, check the scheme, hostname, port, redirects, and exact status code. A 404 may indicate the wrong path or a deliberately hidden resource; it does not necessarily mean the host is down. If curl and the browser behave differently, compare cookies, headers, proxy settings, and authentication state.

For each room question, identify whether it asks for a method, header, status code, cookie, or protocol property. Verify the exact value in the live request or response. Do not copy tokens or invent output; redact flags as `flag{hidden}` in public notes.
