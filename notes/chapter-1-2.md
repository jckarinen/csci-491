# Hypermedia Systems, Chapter 1

## Components of a Hypermedia System

### Anatomy of a URL
Most subcomponents of a URL are not required, and are often omitted.
```
[scheme]://[userinfo]@[host]:[port][path]?[query]#[fragment]
```

### Anatomy of an HTTP request
```
GET /book/contents/ HTTP/1.1
Accept: text/html,*/*
Host: hypermedia.systems
```
An HTTP request contains:
- Type of the request (GET, POST, etc)
- The path of the resource being requested (e.g. `/book/contents/`)
- The HTTP version being used
- A series of HTTP request **headers** (e.g. `Host:`)
  - These provide metadata that helps the server figure out how to respond to the request.

### Anatomy of an HTTP response
```
HTTP/1.1 200 OK
Content-Type: text/html; charset=utf-8
Content-Length: 870
Server: Werkzeug/2.0.2 Python/3.8.10
Date: Sat, 23 Apr 2022 18:27:55 GMT

<html lang="en">
<body>
  <header>
    <h1>HYPERMEDIA SYSTEMS</h1>
  </header>
  ...
</body>
</html>
```
An HTTP response looks similar to a request and contains:
- The HTTP version being used
- A response code, e.g. `200` (success)
- Again, headers that provide metadata to the client
- Finally, the **content:** the HTML representation of the requested resource

### HTTP response codes
- `100 - 199`: Information responses: provide information about how the server is processing the response
- `200 - 299`: Successful responses
- `300 - 399`: Redirection responses: indicate the request should be sent to some other URL
- `400 - 499`: Client error responses (e.g. 404): indicate the client made a bad request (e.g. asking for something that doesn't exist)
- `500 - 599`: Server error responses: indicate the server encountered an internal error as it attempted to respond

### HTTP caching
The HTTP server can instruct the client to hold onto resources for a certain period of time, so that the server does not need to waste resources by serving the same resource repeatedly and unnecessarily.

### HOWL stack
**"HOWL:"** Hypermedia On Whatever you'd Like

Because the hypermedia approach doesn't rely heavily on client-sided JavaScript, it alleviates pressure on the developer to adopt JavaScript-based server technologies like node.js for their backend, a choice that they otherwise may have made in order to have a language-unified codebase.

A hypermedia approach does not have this issue. Since the reliance on client-side JS is eliminated, you are free to use whichever languages and technologies best suit your project and your personal preferences.
  