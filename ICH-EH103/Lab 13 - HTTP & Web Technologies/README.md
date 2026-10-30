# ICH-EH103 — LAB 13: HTTP AND WEB TECHNOLOGIES

**Course:** Iconic Hub Ethical Hacking Course  
**Module:** ICH-EH103 — Web Application Security Fundamentals  
**Lab Number:** 13  
**Topic:** HTTP and Web Technologies  
**Duration:** 3 Hours  
**Practical Environment:** Kali Linux  
**Primary Tools:** Firefox, Kali Linux Terminal, curl, Browser Developer Tools  
**Next Lab:** Lab 14 — Burp Suite Fundamentals

---

# 1. INTRODUCTION

Modern websites and web applications depend on communication between web browsers, web servers, databases, and other services.

Whenever a user visits a website, signs into an account, searches for a product, submits a form, or makes an online purchase, several communication processes occur in the background.

These processes are largely built around web technologies such as:

- Hypertext Transfer Protocol (HTTP).
- Hypertext Transfer Protocol Secure (HTTPS).
- Hypertext Markup Language (HTML).
- Cascading Style Sheets (CSS).
- JavaScript.
- Uniform Resource Locators (URLs).
- Domain Name System (DNS).
- Cookies and sessions.
- Application Programming Interfaces (APIs).
- Web servers and databases.

For an ethical hacker, understanding these technologies is essential because web application vulnerabilities often arise from weaknesses in how browsers, servers, and databases communicate.

Before attempting web application security testing, a penetration tester must understand how web requests are generated, transmitted, processed, and returned to the browser.

This laboratory introduces the fundamental technologies that support modern web applications and explains their relevance to ethical hacking.

# 2. LEARNING OBJECTIVES

By the end of this laboratory, students should be able to:

1. Explain how the World Wide Web works.
2. Understand the client-server architecture.
3. Explain the purpose of HTTP and HTTPS.
4. Identify the components of a URL.
5. Understand HTTP requests and responses.
6. Identify common HTTP request methods.
7. Interpret common HTTP response status codes.
8. Understand HTTP headers and their security relevance.
9. Explain the functions of HTML, CSS, and JavaScript.
10. Understand cookies and session management.
11. Explain how APIs communicate with web applications.
12. Understand the role of DNS, TCP, and TLS in web communication.
13. Use Kali Linux tools to inspect basic HTTP communication.
14. Explain how HTTP knowledge supports web application penetration testing.

# 3. WHAT IS THE WORLD WIDE WEB?

The World Wide Web (WWW) is a system of interconnected resources that users access through the internet.

These resources include:

- Websites.
- Web applications.
- Images.
- Videos.
- Documents.
- Online services.
- Application interfaces.

Users typically access web resources through a web browser such as:

- Google Chrome.
- Mozilla Firefox.
- Microsoft Edge.
- Safari.

The browser communicates with web servers to request and retrieve information.

## 3.1 Internet vs World Wide Web

The internet and the World Wide Web are related but different concepts.

**Internet:** The global network infrastructure that connects computers, servers, and other devices.

**World Wide Web:** A collection of interconnected web resources accessed through the internet using technologies such as HTTP and HTTPS.

The internet provides the communication infrastructure, while the web provides a system for accessing websites and web applications.

# 4. CLIENT-SERVER ARCHITECTURE

Most web applications use a client-server architecture.

This architecture involves two primary components:

## 4.1 Client

A client is a device or application that requests services or resources from a server.

Examples include:

- A computer running Firefox.
- A smartphone running Chrome.
- A web application making an API request.

The client sends requests to the server and processes the responses.

## 4.2 Server

A server is a computer or software application that receives requests, processes them, and returns responses.

A web server may:

- Deliver website files.
- Process user authentication.
- Handle submitted forms.
- Retrieve information from a database.
- Return product information.
- Process application requests.

Examples of web server software include:

- Apache HTTP Server.
- Nginx.
- Microsoft IIS.
- Caddy.

## 4.3 How Client-Server Communication Works

Consider a user visiting an online shopping website.

1. The user enters the website address into a browser.
2. The browser identifies the destination hostname.
3. DNS resolves the hostname to an IP address.
4. The browser establishes the required network connection.
5. For HTTPS, a TLS handshake establishes an encrypted connection.
6. The browser sends an HTTP request.
7. The web server receives and processes the request.
8. The server may communicate with an application or database.
9. The server returns an HTTP response.
10. The browser processes the response and displays the page.

### Client-server communication diagram

```text
+----------------------+
|      USER            |
| Opens a website      |
+----------+-----------+
           |
           v
+----------------------+
|      BROWSER         |
| Chrome / Firefox     |
+----------+-----------+
           |
           | DNS lookup
           v
+----------------------+
|      DNS SERVER      |
| Resolves hostname    |
+----------+-----------+
           |
           v
+----------------------+
|     WEB SERVER       |
| Receives HTTP request|
+----------+-----------+
           |
           v
+----------------------+
|   APPLICATION LOGIC  |
| Processes request    |
+----------+-----------+
           |
           v
+----------------------+
|       DATABASE       |
| Stores application   |
| information          |
+----------+-----------+
           |
           v
+----------------------+
|   HTTP RESPONSE      |
| Returns information  |
+----------+-----------+
           |
           v
+----------------------+
|      BROWSER         |
| Displays web page    |
+----------------------+
```

**Security relevance:** An ethical hacker must understand the communication path because vulnerabilities may exist in the browser, web server, application logic, API, or database.

# 5. UNDERSTANDING HTTP

## 5.1 What is HTTP?

HTTP stands for Hypertext Transfer Protocol.

It is an application-layer protocol used for communication between clients and web servers.

HTTP allows browsers and other clients to request resources and submit information to web servers.

Examples include:

- Requesting a homepage.
- Loading a product image.
- Submitting a login form.
- Searching for products.
- Sending information to an API.

HTTP follows a request-response communication model.

The client sends a request, and the server returns a response.

## 5.2 How HTTP Works

Suppose a user visits:

`http://example.com`

The browser sends a request to the web server.

The server processes the request and returns a response containing information about the requested resource.

A simplified HTTP request looks like this:

```http
GET / HTTP/1.1
Host: example.com
User-Agent: Mozilla/5.0
Accept: text/html
```

The server may respond with:

```http
HTTP/1.1 200 OK
Content-Type: text/html

<!DOCTYPE html>
<html>
<head>
    <title>Example Website</title>
</head>
<body>
    <h1>Welcome</h1>
</body>
</html>
```

The browser interprets the HTML and displays the webpage.

## 5.3 HTTP Is Stateless

HTTP is described as a stateless protocol.

This means that each request is independent at the protocol level. The protocol does not automatically remember previous requests.

For example:

1. A user visits a login page.
2. The user submits login credentials.
3. The server verifies the credentials.
4. The server establishes an authenticated session.
5. The browser sends the session information with subsequent requests.

Cookies and other session mechanisms help web applications maintain user state across multiple HTTP requests.

**Security relevance:** Poor session management can allow attackers to exploit authentication and session-handling weaknesses.

# 6. UNDERSTANDING HTTPS

## 6.1 What is HTTPS?

HTTPS stands for Hypertext Transfer Protocol Secure.

It is HTTP communication protected by Transport Layer Security (TLS).

HTTPS provides important security properties:

**Confidentiality:** Protects transmitted information from being read by unauthorized parties.

**Integrity:** Helps detect unauthorized modification of transmitted data.

**Authentication:** Helps the client verify the identity of the server through its TLS certificate.

HTTPS commonly uses TCP port 443, while ordinary HTTP commonly uses TCP port 80.

## 6.2 HTTP vs HTTPS

| Feature | HTTP | HTTPS |
|---|---|---|
| Full meaning | Hypertext Transfer Protocol | HTTP Secure |
| Encryption | No TLS protection | TLS protection |
| Default port | 80 | 443 |
| Data confidentiality | Not provided by HTTP itself | Provided by TLS |
| Certificate | Not required | Server certificate required |
| Security | Unsuitable for sensitive communication | Preferred for web communication |

## 6.3 Why HTTPS Matters in Ethical Hacking

An ethical hacker examines whether:

- Sensitive information is transmitted over HTTPS.
- HTTP requests are redirected appropriately to HTTPS.
- TLS certificates are valid.
- Secure cookie attributes are configured.
- Sensitive pages are protected against inappropriate caching.
- Security-related response headers are configured correctly.

HTTPS does not automatically make an application secure. SQL injection, XSS, broken authentication, and access-control vulnerabilities can still exist in an HTTPS application.

# 7. UNDERSTANDING URLS

A URL stands for Uniform Resource Locator.

It identifies the location of a resource on the web.

Example:

`https://shopshield.example.com/products?id=25`

A URL contains several components.

```text
https://shopshield.example.com/products?id=25
  |             |                  |      |
  |             |                  |      |
Scheme        Hostname            Path   Query
```

## 7.1 URL Components

**Scheme:** Defines the protocol used to access the resource.

Examples:

- `http`
- `https`

**Hostname:** Identifies the destination server.

Example:

`shopshield.example.com`

**Subdomain:** An additional label preceding the main domain.

Example:

`shopshield`

**Domain:** The registered domain name.

Example:

`example.com`

**Path:** Identifies a particular resource or application route.

Example:

`/products`

**Query string:** Contains parameters sent as part of the URL.

Example:

`?id=25`

## 7.2 URL Parameters

Consider:

`https://shop.example.com/products?id=25`

The query parameter is:

`id=25`

The application may use this value to identify a product.

For example:

- `id=10` may identify a shirt.
- `id=25` may identify a laptop.
- `id=30` may identify a phone.

Security testers examine how applications process user-controlled URL parameters.

Potential weaknesses include:

- Insecure Direct Object Reference (IDOR).
- SQL injection.
- Improper input validation.
- Information disclosure.
- Broken access control.

Changing a parameter is not automatically an attack. The tester must determine whether the application properly enforces authorization and validates input.

# 8. HTTP REQUEST METHODS

HTTP methods indicate the action a client wants a server to perform.

Common HTTP methods include:

## 8.1 GET

GET requests information from a server.

Example:

```http
GET /products HTTP/1.1
Host: example.com
```

This requests the products resource.

GET requests should not be used to perform actions that change application state.

## 8.2 POST

POST submits information to a server.

Example:

```http
POST /login HTTP/1.1
Host: example.com
Content-Type: application/json

{
    "email": "student@example.com",
    "password": "test-password"
}
```

This is an illustrative request showing how a login form might submit data.

POST is commonly used for:

- Login.
- Registration.
- Contact forms.
- Creating records.
- Submitting reviews.

## 8.3 PUT

PUT is generally used to create or replace a resource at a specified location.

Example:

```http
PUT /users/25 HTTP/1.1
Host: example.com
```

## 8.4 PATCH

PATCH is used to apply partial modifications to an existing resource.

Example:

```http
PATCH /users/25 HTTP/1.1
Content-Type: application/json

{
    "name": "Updated User"
}
```

## 8.5 DELETE

DELETE requests removal of a resource.

Example:

```http
DELETE /products/25 HTTP/1.1
Host: example.com
```

This method can change application data and should only be demonstrated against a disposable local lab or authorized test resource.

## 8.6 HEAD

HEAD requests the headers associated with a resource without requesting its response body.

## 8.7 OPTIONS

OPTIONS requests information about the communication options available for a resource.

It is also used in CORS preflight requests.

### Summary of HTTP methods

| Method | Purpose |
|---|---|
| GET | Retrieve information |
| POST | Submit information |
| PUT | Create or replace a resource |
| PATCH | Partially update a resource |
| DELETE | Remove a resource |
| HEAD | Retrieve response headers |
| OPTIONS | Discover supported communication options |

**Ethical hacking relevance:** Understanding HTTP methods helps testers identify how applications expose functionality and whether access controls are consistently enforced.

Do not send PUT, PATCH, or DELETE requests to production systems during this introductory exercise.

# 9. HTTP RESPONSE STATUS CODES

When a server receives an HTTP request, it returns a status code indicating the outcome.

HTTP status codes are divided into five categories.

## 9.1 1xx — Informational

These codes indicate that the request has been received and processing is continuing.

Example:

- `100 Continue`

## 9.2 2xx — Successful

These codes indicate that the request was successfully received, understood, and processed.

Examples:

- `200 OK`
- `201 Created`
- `202 Accepted`
- `204 No Content`

## 9.3 3xx — Redirection

These codes indicate that further action is required to complete the request.

Examples:

- `301 Moved Permanently`
- `302 Found`
- `304 Not Modified`
- `307 Temporary Redirect`
- `308 Permanent Redirect`

## 9.4 4xx — Client Errors

These codes indicate a problem with the request.

Examples:

- `400 Bad Request`
- `401 Unauthorized`
- `403 Forbidden`
- `404 Not Found`
- `405 Method Not Allowed`
- `429 Too Many Requests`

## 9.5 5xx — Server Errors

These codes indicate that the server encountered a problem while processing the request.

Examples:

- `500 Internal Server Error`
- `501 Not Implemented`
- `502 Bad Gateway`
- `503 Service Unavailable`
- `504 Gateway Timeout`

### Important status code distinctions

**401 Unauthorized:** Authentication is required or the supplied authentication is invalid.

**403 Forbidden:** The server understood the request but refuses to authorize it.

**404 Not Found:** The requested resource was not found, or the server is not revealing its existence.

**500 Internal Server Error:** The server encountered an unexpected condition.

**Security relevance:** Unexpected 500 responses may reveal weaknesses or error-handling problems. However, a 500 response alone does not prove a vulnerability.

# 10. HTTP HEADERS

HTTP headers provide additional information about requests and responses.

They help clients and servers communicate information about:

- Content types.
- Authentication.
- Cookies.
- Caching.
- Accepted formats.
- Security policies.
- Browser behavior.

## 10.1 Common Request Headers

**Host**

Identifies the destination hostname.

```http
Host: example.com
```

**User-Agent**

Identifies information about the client software.

```http
User-Agent: Mozilla/5.0
```

**Accept**

Indicates the content types the client can process.

```http
Accept: text/html
```

**Content-Type**

Identifies the media type of the request body.

```http
Content-Type: application/json
```

**Authorization**

Carries authentication credentials or tokens according to the configured authentication scheme.

```http
Authorization: Bearer example-token
```

Never publish or share real authorization tokens.

**Cookie**

Contains cookies previously stored by the browser for the relevant site.

```http
Cookie: session_id=example-session
```

## 10.2 Common Response Headers

**Content-Type**

Identifies the format of the response.

```http
Content-Type: text/html
```

**Set-Cookie**

Instructs the browser to store a cookie.

```http
Set-Cookie: session_id=example-session; Secure; HttpOnly
```

**Cache-Control**

Controls caching behavior.

```http
Cache-Control: no-store
```

**Location**

Identifies a destination used in a redirect.

```http
Location: /account
```

**Strict-Transport-Security**

Instructs browsers to use HTTPS for future requests to the relevant domain.

```http
Strict-Transport-Security: max-age=31536000
```

**Content-Security-Policy**

Defines restrictions on the sources from which a browser may load certain resources.

```http
Content-Security-Policy: default-src 'self'
```

**X-Content-Type-Options**

Helps prevent browsers from interpreting a response as a different content type.

```http
X-Content-Type-Options: nosniff
```

### Security relevance of HTTP headers

Security testers inspect response headers to identify:

- Missing security controls.
- Insecure cookie configuration.
- Weak content security policies.
- Unnecessary information disclosure.
- Inappropriate caching of sensitive content.

A missing header should be evaluated in context rather than automatically classified as a vulnerability.

# 11. WEB TECHNOLOGIES

Modern web applications use multiple technologies that work together.

The three fundamental frontend technologies are HTML, CSS, and JavaScript.

## 11.1 HTML — Hypertext Markup Language

HTML defines the structure and content of a webpage.

It provides elements such as:

- Headings.
- Paragraphs.
- Forms.
- Buttons.
- Images.
- Links.
- Tables.

Example:

```html
<!DOCTYPE html>
<html>
<head>
    <title>ShopShield</title>
</head>
<body>
    <h1>Welcome to ShopShield</h1>

    <p>Online shopping platform.</p>

    <button>Shop Now</button>
</body>
</html>
```

HTML tells the browser what elements should appear on a webpage.

**Security relevance:** Poorly handled HTML can contribute to cross-site scripting when untrusted input is inserted into unsafe rendering contexts.

## 11.2 CSS — Cascading Style Sheets

CSS controls the presentation and appearance of webpages.

It is used to define:

- Colours.
- Fonts.
- Layouts.
- Spacing.
- Borders.
- Responsive designs.
- Animations.

Example:

```css
body {
    background-color: white;
    font-family: Arial, sans-serif;
}

h1 {
    color: blue;
    text-align: center;
}

button {
    padding: 10px;
    background-color: black;
    color: white;
}
```

CSS improves the appearance and usability of web applications.

**Security relevance:** CSS is not normally responsible for SQL injection or XSS, but poorly controlled styles and resource loading may contribute to interface manipulation and information leakage in certain contexts.

## 11.3 JavaScript

JavaScript is a programming language used to add functionality and interactivity to webpages.

It can:

- Validate form inputs.
- Handle button clicks.
- Update webpage content.
- Communicate with APIs.
- Process user interactions.
- Manage frontend application state.

Example:

```javascript
function welcomeUser() {
    alert("Welcome to ShopShield");
}
```

JavaScript can also communicate with a server using APIs and the Fetch API.

Example:

```javascript
fetch("/api/products")
    .then(response => response.json())
    .then(data => {
        console.log(data);
    });
```

**Security relevance:** Improper JavaScript implementation may introduce DOM-based XSS, insecure authentication handling, or exposure of sensitive information.

# 12. WEB APPLICATION ARCHITECTURE

A web application typically contains multiple layers.

## 12.1 Frontend

The frontend is the part of the application that users interact with.

It includes:

- HTML.
- CSS.
- JavaScript.
- Forms.
- Navigation.
- Buttons.
- Product pages.
- User dashboards.

## 12.2 Backend

The backend processes application logic and handles requests received from the frontend.

Its responsibilities may include:

- User authentication.
- Authorization.
- Data validation.
- Business logic.
- Payment processing.
- Database communication.
- API processing.

Backend technologies include:

- Node.js.
- Python.
- PHP.
- Java.
- C#.
- Go.

## 12.3 Database

A database stores application information.

Examples of stored information include:

- User profiles.
- Product records.
- Orders.
- Reviews.
- Contact messages.
- Inventory information.

Common database technologies include:

- PostgreSQL.
- MySQL.
- Microsoft SQL Server.
- MongoDB.
- SQLite.

**Security relevance:** Weaknesses in database queries, access controls, or input handling may lead to SQL injection and unauthorized access to data.

# 13. APIs AND JSON

## 13.1 What is an API?

API stands for Application Programming Interface.

An API defines how different software applications communicate with each other.

Web applications commonly use APIs to exchange data between a frontend and a backend.

For example, an e-commerce application may use an API to:

- Retrieve products.
- Authenticate users.
- Retrieve account information.
- Create orders.
- Submit product reviews.

## 13.2 REST APIs

REST is an architectural style commonly used to design web APIs.

A REST-style application may expose endpoints such as:

```text
GET    /api/products
GET    /api/products/25
POST   /api/orders
GET    /api/orders/10
PATCH  /api/profile
```

These are illustrative endpoint patterns. Actual routes depend on the application.

## 13.3 JSON

JSON stands for JavaScript Object Notation.

It is a lightweight data format commonly used for exchanging information between web applications.

Example:

```json
{
    "id": 25,
    "name": "Laptop",
    "price": 450000,
    "available": true
}
```

JSON uses key-value pairs to represent structured data.

**Security relevance:** Ethical hackers inspect API requests and responses to identify excessive data exposure, weak authorization, insecure input handling, and improper access control.

# 14. COOKIES AND SESSION MANAGEMENT

## 14.1 What are Cookies?

Cookies are small pieces of data stored by a browser and associated with a website.

They allow applications to maintain information across multiple HTTP requests.

Cookies may be used for:

- User sessions.
- Authentication state.
- Shopping cart preferences.
- Language preferences.
- User interface settings.

## 14.2 How Cookies Work

A server may send a cookie through a response header:

```http
Set-Cookie: session_id=example-session; Secure; HttpOnly; SameSite=Lax
```

The browser stores the cookie and sends it with subsequent matching requests.

```http
Cookie: session_id=example-session
```

## 14.3 Important Cookie Attributes

**Secure**

Restricts transmission of the cookie to secure HTTPS connections.

**HttpOnly**

Prevents ordinary JavaScript from reading the cookie through browser APIs such as `document.cookie`.

**SameSite**

Controls when cookies are included in cross-site requests and can help mitigate certain cross-site request forgery (CSRF) scenarios.

**Path**

Defines the URL path scope for which a cookie is sent.

**Domain**

Defines the domain scope for which a cookie is available.

## 14.4 Sessions

A session allows a web application to associate multiple requests with a particular user or client context.

For example:

1. A user logs in.
2. The server authenticates the user.
3. The application establishes an authenticated session.
4. The browser sends the session identifier with later requests.
5. The server uses the session information to identify the authenticated user.

### Common session security weaknesses

- Session fixation.
- Session hijacking.
- Weak session identifiers.
- Missing Secure cookie attributes.
- Missing HttpOnly attributes where appropriate.
- Inadequate session expiration.
- Failure to invalidate sessions after logout.

**Security relevance:** Session management is a major area of web application security and will be explored further in later authentication and access-control exercises.

# 15. DNS, TCP AND TLS IN WEB COMMUNICATION

## 15.1 DNS

DNS stands for Domain Name System.

It translates domain names into IP addresses that network clients can use to reach destination systems.

Example:

```text
example.com → IP address
```

Useful DNS record types include:

| Record | Purpose |
|---|---|
| A | Maps a hostname to an IPv4 address |
| AAAA | Maps a hostname to an IPv6 address |
| CNAME | Provides an alias for another hostname |
| MX | Identifies mail servers |
| NS | Identifies authoritative name servers |
| TXT | Stores text-based DNS information |

## 15.2 TCP

TCP stands for Transmission Control Protocol.

It provides reliable, ordered delivery of data between communicating systems.

HTTP/1.1 and HTTP/2 commonly use TCP as their transport protocol.

## 15.3 TLS

TLS stands for Transport Layer Security.

It protects communications between clients and servers.

TLS provides encryption and helps establish the identity of the communicating server.

HTTP/3 uses QUIC, which is based on UDP and incorporates TLS 1.3.

**Security relevance:** Understanding DNS, TCP, and TLS helps ethical hackers investigate connectivity, certificate problems, and the security of web communication.

# 16. PRACTICAL LABORATORY EXERCISES

**Environment:** Kali Linux  
**Duration:** Approximately 90 minutes  
**Scope:** Basic observation and inspection of HTTP communication.

Only use systems you own or have permission to test. These introductory exercises do not require vulnerability exploitation.

## Exercise 1: Inspect DNS Resolution

**Objective:** Understand how domain names are resolved into IP addresses.

**Tool:** `nslookup`

### Procedure

1. Open Kali Linux.
2. Open the terminal.
3. Execute:

```bash
nslookup example.com
```

4. Observe the DNS server used.
5. Identify the returned IPv4 and IPv6 addresses, if available.
6. Record the results.

### Questions

1. What does DNS do?
2. What is an IPv4 address?
3. What is an IPv6 address?
4. Why do browsers need DNS resolution?

## Exercise 2: Inspect HTTP Response Headers

**Objective:** Identify HTTP response headers and status codes.

**Tool:** `curl`

### Procedure

1. Open the Kali terminal.
2. Execute:

```bash
curl -I https://example.com
```

3. Observe the HTTP response.
4. Identify the status code.
5. Identify the Content-Type header.
6. Identify the server header, if present.
7. Record the results.

### Expected observations

Students should be able to identify:

- HTTP protocol version.
- Response status.
- Date.
- Content-Type.
- Server.
- Other available response headers.

### Questions

1. What does HTTP 200 mean?
2. What is the purpose of Content-Type?
3. Why are response headers important?
4. What is the difference between an HTTP response and an HTTP request?

## Exercise 3: View the Complete HTTP Response

**Objective:** Understand the relationship between response headers and response body.

### Procedure

Execute:

```bash
curl -i https://example.com
```

Observe the output.

Identify:

- Response status line.
- Response headers.
- Blank line separating headers and body.
- HTML response body.

Compare the output with Exercise 2.

## Exercise 4: Inspect an HTTP Request Verbosely

**Objective:** Observe the communication process between a client and server.

Execute:

```bash
curl -v https://example.com
```

Observe:

- Connection establishment.
- TLS handshake information.
- Request method.
- Request headers.
- Server response.
- Connection information.

Do not publish sensitive information from verbose requests if using authenticated applications.

### Questions

1. What is the purpose of the verbose option?
2. Which HTTP method was used?
3. Which headers were sent?
4. What response did the server return?

## Exercise 5: Inspect a Webpage Using Firefox Developer Tools

**Objective:** Understand how browsers process web resources.

### Procedure

1. Open Firefox.
2. Visit `https://example.com`.
3. Press `F12` to open Developer Tools.
4. Select the **Network** tab.
5. Reload the webpage.
6. Observe the list of network requests.
7. Select the main document request.
8. Inspect its request URL, method, status, and headers.
9. Inspect the response headers.
10. Select other resources, such as images or scripts.

### Questions

1. What is the main document request?
2. How many resources were requested?
3. What HTTP method was used?
4. What response status was returned?
5. What is the purpose of the Network tab?

## Exercise 6: Identify HTTP Status Codes

**Objective:** Understand common server response codes.

Use the following commands:

```bash
curl -I https://example.com
```

```bash
curl -I https://example.com/nonexistent-page
```

Compare the responses.

Record the status codes and explain why they differ.

Do not assume that every website will return identical responses for nonexistent routes.

# 17. APPLICATION SECURITY CONNECTION

The technologies covered in this lesson provide the foundation for understanding common web application vulnerabilities.

| Technology | Related security concern |
|---|---|
| HTTP | Insecure communication and request handling |
| HTTPS/TLS | Certificate and transport security |
| HTML | Unsafe content rendering and XSS |
| JavaScript | DOM-based XSS and client-side weaknesses |
| SQL/database | SQL injection |
| URL parameters | IDOR and input validation |
| Cookies | Session security |
| APIs | Broken access control and excessive data exposure |
| HTTP headers | Security misconfiguration |
| DNS | Domain and name-resolution security |

Understanding these relationships prepares students to investigate web applications more effectively.

# 18. INTRODUCTION TO WEB APPLICATION SECURITY TESTING

A web application penetration tester must understand how to:

1. Identify application routes.
2. Observe HTTP requests.
3. Inspect HTTP responses.
4. Identify parameters and input fields.
5. Understand authentication and session behavior.
6. Examine response headers.
7. Inspect API communication.
8. Identify potential security weaknesses.
9. Validate findings within an authorized scope.
10. Document and report security findings.

These activities will be developed further using Burp Suite in Lab 14.

# 19. LABORATORY DELIVERABLES

Each student must submit a report containing the following:

1. Definition of HTTP and HTTPS.
2. Explanation of client-server architecture.
3. A labelled diagram showing how a browser communicates with a web server.
4. Explanation of the components of a URL.
5. Description of at least six HTTP methods.
6. Explanation of the five HTTP status code categories.
7. Description of at least five HTTP request or response headers.
8. Explanation of HTML, CSS, and JavaScript.
9. Explanation of cookies and sessions.
10. Screenshots of the DNS resolution exercise.
11. Screenshots of HTTP response header inspection.
12. Screenshots of Firefox Developer Tools Network inspection.
13. Answers to the practical exercise questions.
14. A short explanation of how HTTP knowledge supports ethical hacking.

### Required screenshot names

```text
lab13_01_dns-resolution.png
lab13_02_http-headers.png
lab13_03_http-response.png
lab13_04_curl-verbose.png
lab13_05_firefox-network.png
lab13_06_request-inspection.png
```

# 20. TAKE-HOME ASSIGNMENT

Answer the following questions:

1. Explain the difference between the internet and the World Wide Web.
2. Explain the client-server architecture.
3. Differentiate between HTTP and HTTPS.
4. Explain why HTTP is considered stateless.
5. Describe the seven HTTP methods discussed in this lesson.
6. Explain the differences between HTTP 401, 403, 404, and 500.
7. What is the purpose of HTTP request and response headers?
8. Explain the difference between HTML, CSS, and JavaScript.
9. Describe how cookies and sessions help web applications maintain user state.
10. Explain the relationship between DNS, TCP, TLS, and HTTP.
11. What is an API, and why is it important in modern web applications?
12. Identify five ways understanding HTTP helps an ethical hacker assess web application security.

# 21. CONCLUSION

HTTP and web technologies form the foundation of modern web application communication.

Understanding how browsers, web servers, APIs, and databases interact is essential for identifying and assessing web application security weaknesses.

Ethical hackers must understand HTTP requests, responses, methods, headers, status codes, cookies, and application architecture before performing more advanced web application security assessments.

This laboratory establishes the theoretical and practical foundation required for the next stage of the course.

# 22. REFERENCES

1. MDN Web Docs. (n.d.). [Overview of HTTP](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Overview).
2. MDN Web Docs. (n.d.). [How the Web Works](https://developer.mozilla.org/en-US/docs/Learn_web_development/Getting_started/Web_standards/How_the_web_works).
3. MDN Web Docs. (n.d.). [HTTP Messages](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Messages).
4. MDN Web Docs. (n.d.). [HTTP Reference](https://developer.mozilla.org/en-US/docs/Web/HTTP).
5. OWASP. (n.d.). [Web Security Testing Guide](https://owasp.org/www-project-web-security-testing-guide/).

---

**ICONIC HUB**

**Secure. Build. Innovate.**

*Where Web Development Meets Cybersecurity.*
