# HTTP in Detail: TryHackMe Walkthrough

Room: https://tryhackme.com/room/httpindetail

HTTP is the application protocol browsers and web services use to exchange requests and responses. HTTPS is HTTP carried over TLS, which protects the connection against eavesdropping and tampering when configured correctly.

## 1. A request has a structure

A typical HTTP request contains a method, path, protocol version, headers, a blank line, and sometimes a body. For example:

```http
GET / HTTP/1.1
Host: example.com
User-Agent: example-client
Accept: */*
```

The method describes the intended operation. GET requests a representation, POST commonly submits data, PUT commonly creates or replaces a resource, PATCH applies a partial update, and DELETE requests removal. Actual server behaviour depends on the application, so a method's name is not proof that the action is safe.

Headers provide metadata. The Host header identifies the requested hostname in HTTP/1.1. Content-Type describes the body format, while Authorization may carry credentials or a token. Cookies can carry session identifiers and must be protected.

## 2. Responses and status codes

A response contains a status line, headers, and optionally a body. Common status codes include:

- **200 OK:** the request succeeded.
- **301 / 302:** redirection.
- **400 Bad Request:** the request was invalid.
- **401 Unauthorized:** authentication is required or failed.
- **403 Forbidden:** the server refuses the request.
- **404 Not Found:** the requested resource was not found or is being hidden.
- **405 Method Not Allowed:** the method is not supported for the resource.
- **500 Internal Server Error:** the server encountered an error.

Read the response body and headers as well as the status code. A 200 response can still contain an application-level error, and a 404 may be intentionally used to conceal whether a resource exists.

## 3. Inspect requests from the command line

Use `curl` to examine a public example or a lab target you are authorised to test:

```bash
curl -i https://example.com/
curl -I https://example.com/
curl -v https://example.com/
```

- `-i` includes response headers in the output.
- `-I` requests headers only, using a HEAD request when supported.
- `-v` shows connection and protocol details useful for troubleshooting.

Replace the example host with the room's target when required. Avoid placing real passwords or API tokens directly in shell history.

To send a form body in a controlled lab, `curl` supports `-d`, but first check the endpoint and the expected content type. Do not send state-changing requests to a system without permission.

## 4. Cookies, sessions, and authentication

HTTP itself is stateless: each request is evaluated independently at the protocol level. Web applications commonly use cookies to associate requests with a server-side session. Session cookies should be protected with Secure and HttpOnly attributes, and an appropriate SameSite policy.

Authentication establishes who the user is. Authorisation determines what that user is allowed to do. A valid session does not mean every record or endpoint should be accessible. The server must check permissions for each sensitive action.

## 5. HTTPS and TLS

HTTPS protects data in transit and helps the client verify the server's identity through certificate validation. It does not protect a server from application vulnerabilities, guarantee that a website is honest, or stop a user from submitting sensitive data to a malicious site with a valid certificate.

Certificate warnings should not be ignored casually. In a lab using a self-signed certificate, follow the lab's instructions and understand the reason for the warning rather than disabling verification globally.

## 6. Common web security mistakes

- Trusting user input without validation or contextual output encoding.
- Building SQL queries by concatenating untrusted input.
- Missing authorisation checks on object IDs.
- Exposing debug information or stack traces.
- Storing session identifiers insecurely.
- Allowing dangerous cross-origin behaviour without a clear need.
- Treating a client-side check as if it enforced server-side access control.

When testing, use the lab scope and document the request, response, expected behaviour, and observed behaviour. A finding should explain the security impact, not merely state that a response looked unusual.

## 7. Work through the room

For each question, identify whether it refers to request methods, headers, response codes, cookies, or HTTPS. Read the exact request or response shown in the task. If a status code appears, interpret it in context rather than assuming it proves the whole operation succeeded.

## What I took from this room

HTTP is easier to troubleshoot when I separate the request, response, headers, body, and transport security. It is also a security boundary: the server must validate input, enforce authorisation, and protect sessions regardless of what the browser displays.