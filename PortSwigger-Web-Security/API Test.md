# 
API Test



### what is API?

An API (Application Programming Interfaces) is a set of rules and protocols that allows different software applications to communicate with one another. It acts as a middleman, taking a request from one system, telling another system what to do, and returning the response back to you.

### How an API Works

Think of an API like a **waiter at a restaurant**:

1.  **You (The Client):** Look at the menu and decide what you want.
    
2.  **The Waiter (The API):** Takes your order, delivers it to the kitchen, and brings back your meal.
    
3.  **The Kitchen (The Server/Database):** Prepares the request behind the scenes—you don't need to know how the kitchen is laid out or how the chef cooks to get your food.
    

### examples of API

APIs make modern digital ecosystems work by allowing systems to talk without sharing their internal code or sensitive infrastructure:

*   **Third-Party Integrations:** When a site lets you "Log in with Google" or shows a embedded Google Map, it uses Google's API to handle the authentication or mapping data safely.
    
*   **Payment Processing:** When you buy something online, the website doesn't handle your credit card directly—it sends the details securely via a payment API (like Stripe or PayPal).
    
*   **Weather & Data Apps:** Your phone’s weather app doesn't measure temperature itself; it calls a weather service API to fetch current weather data for your location.
    

so lets if we know this much about api lets talk about api testing

**API testing** is important as vulnerabilities in APIs may undermine core aspects of a website's confidentiality, integrity, and availability.

**API testing** is a type of software testing that validates the application programming interface directly at the logic layer. Instead of clicking buttons on a user interface (UI), testers or automated scripts send requests directly to the API endpoints and analyze the response.

### API RECON:

**API Reconnaissance (API Recon)** is the process of mapping, identifying, and gathering information about an organization’s APIs—including active, undocumented, legacy, or internal endpoints—before conducting security assessments or penetration testing.

so before we start testing , we first need to find as much information about the API as possible , to discover its attack surface.

To begin, you should identify API endpoints. These are locations where an API receives requests about a specific resource on its server.

```plaintext
GET /api/books HTTP/1.1
Host: example.com
```

the api endpoint in this case is api/books,which would retrive a list of books from a library. and it can be more like api/books/adventure, which would retrive a list of adventure books from a library.

after identifying the endpoints, we need to determine how to interact with them.

## API documentation:

**API documentation** is a comprehensive technical deliverable that explains how to effectively integrate and interact with an Application Programming Interface (API). It serves as both a reference manual and a practical guide for developers, detailing available endpoints, request structures, response formats, authentication requirements, and error codes.Documentation can be in both human-readable and machine-readable forms

### Discovering API documentation

Even if API documentation isn't openly available, you may still be able to access it by browsing applications/tools that use the API.

we can try brute force using tools and word lists to try different word or directories or we can use build in apps like burpsuite and try to get the api endpoint.

### `lab 1: exploiting an api endpoint using documentation`

#### Required knowledge

To solve this lab, you'll need to know:

*   What API documentation is.
    
*   How API documentation may be useful to an attacker.
    
*   How to discover API documentation.
    

These points are covered in our [API Testing](https://portswigger.net/web-security/api-testing) Academy topic

solution:

*   In Burp's browser, log in to the application using the credentials `wiener:peter` and update your email address. or just login using the browser .
    

![](https://cdn.hashnode.com/uploads/covers/6a77779d4416d3eea9accad0/769576fe-41ff-449f-af77-e22538f22507.png align="center")

*   In **Proxy > HTTP history**, right-click the `PATCH /api/user/wiener` request and select **Send to Repeater**.
    
*   Go to the **Repeater** tab. Send the `PATCH /api/user/wiener` request. Notice that this retrieves credentials for the user `wiener`.
    
*   Remove `/wiener` from the path of the request, so the endpoint is now `/api/user`, then send the request. Notice that this returns an error because there is no user identifier.
    
*   Remove `/user` from the path of the request, so the endpoint is now `/api`, then send the request. Notice that this retrieves API documentation.
    
*   Right-click the response and select **Show response in browser**. Copy the URL.
    
*   Paste the URL into Burp's browser to access the documentation. Notice that the documentation is interactive.
    

*   To delete Carlos and solve the lab, click on the `DELETE` row, enter `carlos`, then click **Send request**. on the pic i already deleted the user carlos thats why its unauthorized.
    

![](https://cdn.hashnode.com/uploads/covers/6a77779d4416d3eea9accad0/22fd8aaa-513c-4cd0-8982-6f133ca2b157.png align="center")

![](https://cdn.hashnode.com/uploads/covers/6a77779d4416d3eea9accad0/f827bba8-f5cb-4884-93f4-2bc010fc45d8.png align="center")

or can do it on burp too

![](https://cdn.hashnode.com/uploads/covers/6a77779d4416d3eea9accad0/ea794108-e435-4021-a859-66908b001c99.png align="center")

### **Identifying API endpoints:**

**Identifying API endpoints** is the process of locating, mapping, and discovering all the specific URL routes exposed by a backend server that accept network requests.we can also gather a lot of information by browsing applications that use the API. This is often worth doing even if we have access to API documentation, as sometimes documentation may be inaccurate or out of date.

while looking for api endpoints do not forget to check javascript files,These can contain references to API endpoints that you haven't triggered directly via the web browser.

The HTTP method specifies the action to be performed on a resource. For example:

*   `GET` - Retrieves data from a resource.
    
*   `PATCH` - Applies partial changes to a resource.
    
*   `OPTIONS` - Retrieves information on the types of request methods that can be used on a resource.
    

An API endpoint may support different HTTP methods. It's therefore important to test all potential methods when you're investigating API endpoints. This may enable you to identify additional endpoint functionality, opening up more attack surface.

#### **Identifying supported content types**

API endpoints often expect data in a specific format. They may therefore behave differently depending on the content type of the data provided in a request. Changing the content type may enable you to:

*   Trigger errors that disclose useful information.
    
*   Bypass flawed defenses.
    
*   Take advantage of differences in processing logic. For example, an API may be secure when handling JSON data but susceptible to injection attacks when dealing with XML.
    

To change the content type, modify the `Content-Type` header, then reformat the request body accordingly. we can use the [Content type converter](https://portswigger.net/bappstore/db57ecbe2cb7446292a94aa6181c9278) BApp to automatically convert data submitted within requests between XML and JSON.

### `lab2: finding and exploiting an unused API end point`

#### Required knowledge

To solve this lab, you'll need to know:

*   How to use error messages to construct a valid request.
    
*   How HTTP methods are used by RESTful APIs.
    
*   How changing the HTTP method can reveal additional functionality.
    

These points are covered in our [API Testing](https://portswigger.net/web-security/api-testing) Academy topic.

solution:

*   In Burp's browser, access the lab and click on a product.
    
*   In **Proxy > HTTP history**, notice the API request for the product. For example, `/api/products/3/price`.
    

![](https://cdn.hashnode.com/uploads/covers/6a77779d4416d3eea9accad0/def783f9-0eb2-451c-aa0b-c058addc1990.png align="center")

*   Right-click the API request and select **Send to Repeater**.
    
*   In the **Repeater** tab, change the HTTP method for the API request from `GET` to `OPTIONS`, then send the request. Notice that the response specifies that the `GET` and `PATCH` methods are allowed.
    

![](https://cdn.hashnode.com/uploads/covers/6a77779d4416d3eea9accad0/48d6f5a6-b593-4b01-b052-f072f55f97a8.png align="center")

*   Change the method for the API request from `GET` to `PATCH`, then send the request. Notice that you receive an `Unauthorized` message. This may indicate that you need to be authenticated to update the order.
    

![](https://cdn.hashnode.com/uploads/covers/6a77779d4416d3eea9accad0/c59ade4e-25fd-4b9e-b5c0-bcfc81ac74a2.png align="center")

*   In Burp's browser, log in to the application using the credentials `wiener:peter`.
    
*   Click on the **Lightweight "l33t" Leather Jacket** product.
    
*   In **Proxy > HTTP history**, right-click the `API/products/1/price` request for the leather jacket and select **Send to Repeater**.
    
*   In the **Repeater** tab, change the method for the API request from `GET` to `PATCH`, then send the request. Notice that this causes an error due to an incorrect `Content-Type`. The error message specifies that the `Content-Type` should be `application/json`.
    
*   Add a `Content-Type` header and set the value to `application/json`.
    
*   Add an empty JSON object `{}` as the request body, then send the request. Notice that this causes an error due to the request body missing a `price` parameter.
    
*   Add a `price` parameter with a value of `0` to the JSON object `{"price":0}`. Send the request.
    
*   In Burp's browser, reload the leather jacket product page. Notice that the price of the leather jacket is now `$0.00`.
    
*   Add the leather jacket to your basket.
    
*   Go to your basket and click **Place order** to solve the lab.
    

## **Finding hidden parameters:**

**Finding hidden parameters** (also known as parameter discovery or parameter fuzzing) is the process of identifying unlinked, undocumented, or hidden inputs—such as query parameters (`?debug=true`), form data fields, or JSON keys—that an API endpoint or web application accepts and processes behind the scenes.

While endpoint discovery reveals *where* to send requests (e.g., `POST /api/v1/user/profile`), parameter discovery reveals *what arguments* you can pass to that route to alter application behavior, bypass access controls, or uncover vulnerabilities.

### Mass assignment vulnerabilities :

**Mass assignment** (also known as *Auto-Binding* or *Over-Posting*) is a web application vulnerability that occurs when a framework automatically binds client-provided input parameters directly to internal data models or database objects without filtering out restricted fields.

EXAMPLE

The Real-World Analogy: Filling Out a Form Imagine you go to the bank to apply for a standard savings account.

What we suppposed to enter or like should be is this

*   Name
    
*   Email
    
*   Phone Number
    

but the Sneaky Trick is Before handing the paper back to the clerk, i pull out a pen and add a new box at the bottom of the form :

Account Balance: $1,000,000

The Flaw: If the bank clerk takes your paper and types every single box on the page directly into the system without checking, i suddenly have a million dollars.

That is Mass Assignment. The system blindly takes every piece of information you give it and saves it straight to the database.

A Simple Code Example

What a normal user sends:

```json
 JSON { "name": "Alex",
 "email": "alex@gmail.com" } 
```

What a hacker sends (adding hidden fields):

```json
JSON { "name": "Alex",
 "email": "alex@gmail.com",
 "is_admin": true }
```

JSON { "name": "Alex", "email": "alex@gmail.com", "is\_admin": true }

### The Vulnerable Code (Bad Practice):

```python
#The backend code takes EVERYTHING sent in the requestand saves it straight to the user account.
user.update(request.json)
```

user.update(request.json) Because the backend code didn't filter out "is\_admin", Alex just became an admin!

### The Safe Code (Good Practice):

```python
#The backend ONLY accepts name and email.
#Anything else sent in the request is completely ignored.
user.name = request.json["name"] user.email = request.json["email"]
```

### LAB 3: exaploiting a mass assignment vulnerability:

#### **Required knowledge**

To solve this lab, you'll need to know:

*   What mass assignment is.
    
*   Why mass assignment may result in hidden parameters.
    
*   How to identify hidden parameters.
    
*   How to exploit mass assignment vulnerabilities.
    

These points are covered in our [API Testing](https://portswigger.net/web-security/api-testing) Academy topic.

solution:

1.  In Burp's browser, log in to the application using the credentials `wiener:peter`.
    

![](https://cdn.hashnode.com/uploads/covers/6a77779d4416d3eea9accad0/8582f8f5-a7d1-4de0-bf30-cdd43795e0df.png align="center")

1.  Click on the **Lightweight "l33t" Leather Jacket** product and add it to your basket.
    
2.  Go to your basket and click **Place order**. Notice that you don't have enough credit for the purchase.
    
3.  In **Proxy > HTTP history**, notice both the `GET` and `POST` API requests for `/api/checkout`.
    

![](https://cdn.hashnode.com/uploads/covers/6a77779d4416d3eea9accad0/7e0253b7-1ec3-4ae5-b3da-b6b264c06267.png align="center")

1.  Notice that the response to the `GET` request contains the same JSON structure as the `POST` request. Observe that the JSON structure in the `GET` response includes a `chosen_discount` parameter, which is not present in the `POST` request.
    

![](https://cdn.hashnode.com/uploads/covers/6a77779d4416d3eea9accad0/2266f23f-9bb1-4fbc-883d-0f80e46b9fac.png align="center")

1.  Right-click the `POST /api/checkout` request and select **Send to Repeater**.
    
2.  In Repeater, add the `chosen_discount` parameter to the request. The JSON should look like the following:
    
    ```python
    {
        "chosen_discount":{
            "percentage":0
        },
        "chosen_products":[
            {
                "product_id":"1",
                "quantity":1
            }
        ]
    }
    ```
    

1.  Send the request. Notice that adding the `chosen_discount` parameter doesn't cause an error.
    
2.  Change the `chosen_discount` value to the string `"x"`, then send the request. Observe that this results in an error message as the parameter value isn't a number. This may indicate that the user input is being processed.
    

![](https://cdn.hashnode.com/uploads/covers/6a77779d4416d3eea9accad0/8af0cde5-4ade-4944-acd3-15dc79f51dc2.png align="center")

1.  Change the `chosen_discount` percentage to `100`, then send the request to solve the lab.
    

![](https://cdn.hashnode.com/uploads/covers/6a77779d4416d3eea9accad0/da6326ff-9b52-4086-b993-a6b4e28f6470.png align="center")

so that how i did this labs

![]( align="center")

![](https://cdn.hashnode.com/uploads/covers/6a77779d4416d3eea9accad0/ff694be0-a58d-4409-84e0-14ec3ec1613d.png align="center")

***Thank you for reading!***

## Expanded API testing methodology

### 1. Map the API surface

Start from the in-scope application and identify API routes through the browser's Network panel, JavaScript references, OpenAPI/Swagger documentation, or requests supplied by the lab. Record the HTTP method, path, content type, authentication context, and purpose. Do not assume a route is safe because it is undocumented.

A route inventory can use this structure:

| Method and path | Purpose | Auth context | User-controlled fields | Expected permission |
|---|---|---|---|---|
| GET /resource | Read resource | Test user | Object ID | Owner or approved role |
| POST /resource | Create resource | Test user | Request body | Authorized user |
| PATCH /resource/{id} | Modify resource | Test user | ID and fields | Owner or approved role |

Replace these examples with endpoints actually observed in the lab.

### 2. Preserve and replay a baseline request

Capture a legitimate request in Burp Suite and send it to Repeater. Identify which headers carry authentication, which fields select an object, and which parts the server appears to validate. Keep one unmodified baseline so changes can be compared one at a time.

### 3. Test authorization by object and function

Check whether the server verifies that the authenticated identity may access the requested object. In a lab with two test accounts, compare the request made by each account. A sequential or unpredictable ID is not a substitute for authorization. Also check whether a low-privileged account can invoke a privileged function directly even when the UI hides the relevant button.

### 4. Test input validation and API semantics

Determine how the API handles unexpected types, extra JSON properties, missing fields, duplicate fields, unsupported methods, and invalid state transitions. Avoid sending destructive values. For each test, record the baseline response, modified request, observed status and body, and any state change that proves impact.

### 5. Review mass assignment and data exposure

Compare the fields returned by the API with those required by the client. Check whether server-side protected fields such as role, owner, price, or approval state can be supplied by a client. Do not assume that a field is writable just because it appears in a response. Verify the effect in the lab.

### 6. Troubleshooting

If a request behaves differently in Repeater, compare cookies, authorization headers, content type, origin, CSRF token, and request body. A 401 often signals missing or invalid authentication; 403 may indicate insufficient permission, but applications do not always follow this convention. Inspect the response and server behaviour.

### 7. Remediation

Apply object-level and function-level authorization on every request, validate schemas server-side, allowlist writable properties, minimize returned data, rate-limit sensitive operations, and log high-risk changes. Use explicit API versioning and maintain an inventory of exposed routes.

### 8. Evidence and task answers

For each PortSwigger lab, map the answer to the exact endpoint, request modification, and observed impact. Keep the reproducer minimal and non-destructive. Do not invent a status code or response; it depends on the lab instance. Redact session tokens and flags in public notes.
