# HTTP in Detail — TryHackMe

Room: https://tryhackme.com/room/httpindetail

HTTP is the protocol browsers and web servers use to exchange requests and responses. When I open a website, the browser sends a request and the server replies with a status code, headers, and sometimes a body containing HTML or other data.

## A request and response

A request typically includes a method, path, protocol version, headers, and sometimes a body. Common methods include:

- `GET` to retrieve a resource.
- `POST` to submit data.
- `PUT` to create or replace a resource.
- `DELETE` to request removal of a resource.
- `HEAD` to request headers without the normal response body.

The server responds with a status code. For example, `200` usually means success, `301` or `302` indicates a redirect, `400` indicates a bad request, `401` relates to authentication, `403` means the server refuses the request, and `404` means the resource was not found.

## Headers, cookies, and HTTPS

Headers carry extra information about the request or response. A `Content-Type` header describes the media type, while `User-Agent` identifies the client software. Cookies can help a website maintain a session or store preferences. Session cookies need careful handling because someone who steals a valid session token may be able to act as that user.

HTTPS is HTTP protected by TLS. It helps protect traffic in transit and lets the browser authenticate the server using certificates. HTTPS does not automatically make a website safe: a vulnerable application can still send malicious content over an encrypted connection.

## Inspecting traffic

In an authorised lab, browser developer tools or a proxy can show the exact requests and responses. I should look at the method, URL, parameters, cookies, status code, and response headers. A request that fails is still useful because the response may explain whether the problem is authentication, permissions, or a missing route.

## What I took from this room

HTTP is not just a webpage appearing on screen. It is a structured conversation between client and server. Understanding methods, status codes, headers, cookies, and HTTPS gives me a foundation for web debugging and for recognising common web-security mistakes.