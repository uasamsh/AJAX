================================================================================
                          AJAX COMPLETE TUTORIAL
                    read.txt - Your Quick Reference Guide
================================================================================

WHAT IS AJAX?
-------------
AJAX = Asynchronous JavaScript And XML

AJAX is NOT a programming language. It is a technique that uses a combination of:
  - A browser built-in XMLHttpRequest object (to request data from a web server)
  - JavaScript and HTML DOM (to display or use the data)

AJAX allows web pages to be updated asynchronously by exchanging data with
a web server behind the scenes. This means it is possible to update parts of
a web page, without reloading the whole page.

================================================================================
                              HOW AJAX WORKS
================================================================================

Step-by-Step Flow:
------------------

  1. An event occurs in a web page (the page is loaded, a button is clicked)
  2. An XMLHttpRequest object is created by JavaScript
  3. The XMLHttpRequest object sends a request to a web server
  4. The server processes the request
  5. The server sends a response back to the web page
  6. The response is read by JavaScript
  7. Proper action (like page update) is performed by JavaScript

Visual Diagram:
---------------

    +----------------+          Request           +----------------+
    |                | -------------------------> |                |
    |   Browser      |   (GET / POST / etc.)      |    Server      |
    |   (JavaScript) |                            |   (PHP/Node/   |
    |                | <------------------------- |    Python/etc) |
    |                |          Response          |                |
    +----------------+                            +----------------+

================================================================================
                         XMLHttpRequest OBJECT
================================================================================

The XMLHttpRequest object is used to exchange data with a server behind
the scenes. It is supported in ALL modern browsers.

Creating an XMLHttpRequest Object:
----------------------------------

    let  xhttp = new XMLHttpRequest();

================================================================================
                         AJAX METHODS (Quick Ref)
================================================================================

Method                      | Description
----------------------------|------------------------------------------------
new XMLHttpRequest()        | Creates a new XMLHttpRequest object
abort()                     | Cancels the current request
getAllResponseHeaders()     | Returns header information
getResponseHeader()         | Returns specific header information
open(method, url, async)    | Specifies the type of request
                            |   method: GET or POST
                            |   url: the server file location
                            |   async: true (async) or false (sync)
send()                      | Sends the request to the server
                            |   Used for GET requests
send(string)                | Sends the request to the server
                            |   Used for POST requests
setRequestHeader()          | Adds a label/value pair to the header
                            |   to be sent with the request

================================================================================
                       AJAX PROPERTIES (Quick Ref)
================================================================================

Property                    | Description
----------------------------|------------------------------------------------
onload                      | Defines a function to be called when
                            |   the request is received (loaded)
onreadystatechange          | Defines a function to be called when
                            |   the readyState property changes
readyState                  | Holds the status of the XMLHttpRequest
                            |   0: request not initialized
                            |   1: server connection established
                            |   2: request received
                            |   3: processing request
                            |   4: request finished and response is ready
responseText                | Returns the response data as a string
responseXML                 | Returns the response data as XML data
status                      | Returns the status-number of a request
                            |   200: "OK"
                            |   403: "Forbidden"
                            |   404: "Not Found"
                            |   500: "Internal Server Error"
statusText                  | Returns the status-text (e.g. "OK" or "Not Found")

================================================================================
                         AJAX GET REQUEST EXAMPLE
================================================================================

Example 1: Basic GET Request
----------------------------

    function loadDoc() {
       let  xhttp = new XMLHttpRequest();

        xhttp.onload = function() {
            document.getElementById("demo").innerHTML = this.responseText;
        }

        xhttp.open("GET", "AJAX_info.txt");
        xhttp.send();
    }

HTML:

    <div id="demo">
        <h2>Let AJAX change this text</h2>
        <button type="button" onclick="loadDoc()">Change Content</button>
    </div>

================================================================================
                        AJAX POST REQUEST EXAMPLE
================================================================================

Example 2: POST Request with Data
---------------------------------

    function sendData() {
        let  xhttp = new XMLHttpRequest();

        xhttp.onload = function() {
            document.getElementById("result").innerHTML = this.responseText;
        }

        xhttp.open("POST", "demo_post.php");
        xhttp.setRequestHeader("Content-type", "application/x-www-form-urlencoded");
        xhttp.send("fname=Henry&lname=Ford");
    }

================================================================================
                   AJAX with onreadystatechange (Old Way)
================================================================================

Example 3: Using onreadystatechange
-----------------------------------

    function loadDoc() {
        const xhttp = new XMLHttpRequest();

        xhttp.onreadystatechange = function() {
            if (this.readyState == 4 && this.status == 200) {
                document.getElementById("demo").innerHTML = this.responseText;
            }
        };

        xhttp.open("GET", "ajax_info.txt", true);
        xhttp.send();
    }

Note: onload is the modern way. It replaces onreadystatechange.

================================================================================
                        MODERN FETCH API (Recommended)
================================================================================

The Fetch API is the modern replacement for XMLHttpRequest.
It uses Promises and is cleaner to write.

Example 4: Fetch API - GET Request
----------------------------------

    fetch('https://api.example.com/data')
        .then(response => {
            if (!response.ok) {
                throw new Error('Network response was not ok');
            }
            return response.text();  // or response.json() for JSON data
        })
        .then(data => {
            console.log(data);
            document.getElementById("demo").innerHTML = data;
        })
        .catch(error => {
            console.error('There was a problem:', error);
        });

Example 5: Fetch API - POST Request
-----------------------------------

    fetch('https://api.example.com/submit', {
        method: 'POST',
        headers: {
            'Content-Type': 'application/json',
        },
        body: JSON.stringify({
            name: 'John',
            age: 30
        })
    })
    .then(response => response.json())
    .then(data => console.log(data))
    .catch(error => console.error('Error:', error));

Example 6: Fetch with async/await (Best Practice)
-------------------------------------------------

    async function fetchData() {
        try {
            const response = await fetch('https://api.example.com/data');

            if (!response.ok) {
                throw new Error(`HTTP error! status: ${response.status}`);
            }

            const data = await response.json();
            console.log(data);
            return data;

        } catch (error) {
            console.error('Fetch error:', error);
        }
    }

================================================================================
                         AJAX with JSON
================================================================================

JSON (JavaScript Object Notation) is the most common format for AJAX data.

Example 7: Parsing JSON Response
--------------------------------

    const xhttp = new XMLHttpRequest();

    xhttp.onload = function() {
        const myObj = JSON.parse(this.responseText);
        document.getElementById("demo").innerHTML = myObj.name;
    }

    xhttp.open("GET", "json_demo.txt", true);
    xhttp.send();

Example 8: Sending JSON Data
----------------------------

    const myObj = { name: "John", age: 31, city: "New York" };
    const myJSON = JSON.stringify(myObj);

    const xhttp = new XMLHttpRequest();
    xhttp.open("POST", "json_demo_db_post.php", true);
    xhttp.setRequestHeader("Content-type", "application/json");
    xhttp.send(myJSON);

================================================================================
                         COMMON USE CASES
================================================================================

1. Auto-Save Forms
   ----------------
   Save form data automatically as the user types, without submitting the form.

2. Live Search Suggestions
   ------------------------
   Show search suggestions as the user types in a search box.

3. Infinite Scroll
   ----------------
   Load more content as the user scrolls down the page.

4. Real-time Updates
   ------------------
   Update news feeds, stock prices, or chat messages without refreshing.

5. Form Validation
   ----------------
   Check if a username is available before submitting the form.

6. Shopping Cart Updates
   ----------------------
   Add items to cart without reloading the page.

================================================================================
                         IMPORTANT NOTES
================================================================================

CORS (Cross-Origin Resource Sharing):
-------------------------------------
- By default, browsers block AJAX requests to different domains.
- The server must include proper CORS headers to allow cross-origin requests.
- Example header: Access-Control-Allow-Origin: *

Async vs Sync:
--------------
- Always use async=true (default) for AJAX requests.
- async=false is deprecated and blocks the browser until the request completes.

Error Handling:
---------------
- Always check response.status or response.ok before using the data.
- Use try/catch with async/await for better error handling.

================================================================================
                         QUICK CHEAT SHEET
================================================================================

| Task                  | XMLHttpRequest Way          | Fetch API Way              |
|-----------------------|-----------------------------|----------------------------|
| Create request        | new XMLHttpRequest()        | fetch(url)                 |
| GET request           | open("GET", url); send()    | fetch(url)                 |
| POST request          | open("POST", url); send(data)| fetch(url, {method:'POST'})|
| Handle response       | onload / onreadystatechange | .then() / await            |
| Parse JSON            | JSON.parse(responseText)    | response.json()            |
| Check status          | this.status == 200          | response.ok                |
| Set headers           | setRequestHeader()          | headers: {} in options     |
| Error handling        | Check status manually       | .catch() / try-catch       |

================================================================================
                         SAMPLE TEST CONTENT
================================================================================

Below is some plain text content you can use to test your AJAX requests:

--- START OF TEST CONTENT ---

Hello! This is a test response from the server.
If you are reading this, your AJAX request worked perfectly!

The quick brown fox jumps over the lazy dog.
Pack my box with five dozen liquor jugs.
How vexingly quick daft zebras jump!

Technology is best when it brings people together.
Code is like humor. When you have to explain it, it's bad.

--- END OF TEST CONTENT ---

================================================================================
                              END OF TUTORIAL
================================================================================


