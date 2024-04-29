---
author: Andrew Xia
pubDatetime: 2024-04-29T14:44:04.400Z
modDatetime: 2024-04-29T14:44:04.400Z
title: A simple introduction about html and js
slug: A simple introduction about html and js
featured: true
draft: false
tags:
  - docs
description: A extremely simple guide about html with js
---

# First step to frontend

<aside>
💡 **Credit to ChatGPT 4.0**

</aside>

### **How does the browser get html and handle the html?**

The process of a browser retrieving a webpage involves the following key steps:

1. **URL Parsing**: When you enter a URL (Uniform Resource Locator) in the browser's address bar and press enter, the browser first parses the URL to determine the protocol (such as HTTP or HTTPS), server address (domain name), and the path to the resource.
2. **DNS (domain name system) Lookup to get the IP address**: 
    1. Browser DNS Cache
    2. Configured DNS Server
3. **Establishing a Connection**: 
    1. TCP for HTTP; TLS (transport layer security) + TCP for HTTPS
4. **Sending an HTTP Request**: After the connection is established, the browser constructs an HTTP request message, which includes a request line (e.g., GET /index.html HTTP/1.1), request headers (e.g., User-Agent, Accept), and a request body (for requests like POST that submit data). The browser then sends this request message to the server.
5. **Server Processing**: Upon receiving the browser's HTTP request, the server processes the request based on the resource path, method, and other information. The server may access a database, execute server-side scripts, or perform other operations to generate the response.
6. **Sending an HTTP Response**: The server packages the processing result into an HTTP response message, which includes a status line (e.g., HTTP/1.1 200 OK), response headers (e.g., Content-Type, Set-Cookie), and a response body (the actual content of the resource). The server then sends this response back to the browser.
7. **Browser Processing Response**: After receiving the HTTP response, the browser first parses the response headers to determine how to handle the content in the response body. If it's an HTML document, the browser starts parsing the HTML and rendering the page, as previously described. If it's a redirect, the browser automatically makes a new HTTP request to the URL specified in the Location header.
    - **Content-Type:** how to handle the content in the response? (TODO)
    - HTML parsing + CSSOM Construction + Javascript Processing + render t
8. **Resource Loading**: The HTML document may include references to other resources like CSS files, JavaScript scripts, images, etc. The browser parses these resource URLs and makes additional HTTP requests to fetch them. The fetching and processing of these resources are done in parallel and asynchronously to improve page load efficiency

### **Coding a simple HTML**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Simple HTML Page</title> 
</head>
<body>
    <h1>Hello, World!</h1>
    <p>This is a simple HTML document.</p>
</body>
</html>
```

**`<head>`**: Contains meta-information about the document, like character set (**`<meta charset="UTF-8">`**), viewport settings for responsive design (**`<meta name="viewport" content="width=device-width, initial-scale=1.0">`**), and the document's title **`<title>`** which is display in the window tab

### **Add JavaScript in HTML**

- Inline JavaScript (Synchronous Execution)
    
    ```html
    <!DOCTYPE html>
    <html>
    <head>
        <title>External JavaScript Example</title>
    </head>
    <body>
    
    <h1>JavaScript in HTML</h1>
    <p id="demo">JavaScript can change HTML content.</p>
    
    <script>
        document.getElementById("demo").innerHTML = "Hello, JavaScript!";
    </script>
    
    </body>
    </html>
    ```
    
- External JavaScript (Both Synchronous and asynchronous)
    
    ```html
    <!DOCTYPE html>
    <html>
    <head>
        <title>External JavaScript Example</title>
    </head>
    <body>
    
    <h1>JavaScript in HTML</h1>
    <p id="demo">JavaScript can change HTML content.</p>
    <!-- import js from other places -->
    <script src="example.js"></script>
    <script async src="example.js"></script>
    <script defer src="example.js"></script>
    
    </body>
    </html>
    ```
    
    ```jsx
    // content of example.js, another file
    document.getElementById("demo").innerHTML = "Hello, JavaScript from an external file!";
    ```
    

<aside>
💡 Best Practices:

- **Placing `<script>` Tags**: It's generally recommended to place external **`<script>`** tags just before the closing **`</body>`** tag. This practice helps improve page load times because it allows the browser to render the HTML content before loading and executing the JavaScript, reducing the chance of a visible delay in rendering the page content.

- **Async and Defer Attributes**: For external scripts, you can use the **`async`** or **`defer`** attributes to control how and when the script is loaded. **`async`** allows the script to be downloaded asynchronously and executed as soon as it's downloaded, while **`defer`** delays script execution until after the HTML document has been fully parsed.
</aside>

### Modify DOM using Javascript

```html
<!DOCTYPE html>
<html>

<head>
    <title>JavaScript In Html Example</title>

</head>

<body>

    <h1>JavaScript in HTML</h1>

    <div id="demo">
        <button onclick="addElement()">
            Add something
        </button>
    </div>

    <script src="utils.js"></script>

</body>

</html>
```

```jsx
const addElement = () => {

    const element = document.getElementById('demo'); // get a html element with id "demo"

    const newDiv = document.createElement('div'); // create a html element, this just create an element and will not attach it to the DOM

    newDiv.innerHTML = 'new div'
    element.appendChild(newDiv); // attach the newDiv to the DOM (because demo is already rendered in the DOM)
}
```

### Add api request in javaScript

```jsx
// utils.js

let count = 0;

const addElement = (text) => {
    const element = document.getElementById('demo');

    const newDiv = document.createElement('div');
		
    // add content and apply styles to the div element
    newDiv.innerHTML = text || 'empty user';
    newDiv.style.padding = '10px';
    newDiv.style.margin = "5px 0px";
    newDiv.style.color = 'white';
    newDiv.style.backgroundColor = count % 2 === 0 ? 'red' : 'blue';
    newDiv.style.width = '200px'
    newDiv.style.borderRadius = '10px';
    newDiv.style.border = "2px solid gray"

    element.appendChild(newDiv);
    count++;
}

const fetchSomeData = async () => {
    const res = await fetch(
        'https://random-data-api.com/api/v2/users/?size=1', {method: 'GET'}
    )

    const data = await res.json();

    addElement(data.username);
}
```

### Introduce React

- How can we create a React project?
    
    Method 1: use `create-react-app` template `npx create-react-app your-app-name`
    
    Method 2: config the project from scratch, refer to [How to create react application from scratch?](https://shuibitianantian.github.io/andrew/posts/how%20to%20create%20new%20react%20project%20from%20scratch) 
    
- How can we start the project?
    
    `npm start`
