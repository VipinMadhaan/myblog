# How to send GET and POST Ajax API requests in Vanilla JavaScript


Ajax is an asynchronous JavaScript and XML mechanism for updating a portion of a page.
It allows you to update a portion of the page after receiving data from the server, avoiding the need to refresh the entire page.

Furthermore, by integrating partial page refreshes in this manner, we not only provide an excellent user experience but also reduce the server burden.

### A simple Ajax request

```js
var xmlhttp = new XMLHttpRequest();
xmlhttp.open('GET', 'send-ajax-request-url');
xmlhttp.send(null);
```

In this case, we first create an instance of a class that can send HTTP requests to the server. Then invoke its open method, passing the HTTP request method as the first parameter and the URL of the request page as the second. Finally, we invoke the send method with no parameters.
If you use the POST request method, the send method parameters should contain any data you want to send.

### Here's how we handle the server's response

```js
xmlhttp.onreadystatechange = function () {
  if (xmlhttp.readyState !== 4) return;
  if (xmlhttp.status === 200) {
    console.log('Request Response', xmlhttp.responseText);
  } else {
    console.log('HTTP error', xmlhttp.status, xmlhttp.statusText);
  }
};
```

Because onreadystatechange is asynchronous, it can be called at any time. This is known as a "callback function", and it is called once the client starts sending a request.

Many people rely on jQuery because of the Ajax method's convenience. But JavaScript's native Ajax API is also very powerful and has similar basic functionality across browsers, allowing you to completely encapsulate an Ajax object.

### An example of a POST Ajax Request

```js
var xmlhttp;
if (window.XMLHttpRequest) {
  xmlhttp = new XMLHttpRequest();
} else {
  xmlhttp = new ActiveXObject('Microsoft.XMLHTTP');
}
xmlhttp.open('POST', 'send-ajax-request-url', true);

// set content type according to your data type
xmlhttp.setRequestHeader('Content-Type', 'application/json');
xmlhttp.setRequestHeader('cache-control', 'no-cache');

// if data type is json use JSON stringify
xmlhttp.send(JSON.stringify(data));

//set request timeout to handle any network error
xmlhttp.timeout = 5000;
xmlhttp.ontimeout = function () {
  console.log('Request Timeout', xmlhttp.responseURL);
};

xmlhttp.onreadystatechange = function () {
  if (xmlhttp.readyState !== 4) return;
  if (xmlhttp.status === 200) {
    let response = '';
    try {
      response = JSON.parse(xmlhttp.responseText);
    } catch (e) {
      response = xmlhttp.responseText;
    }
    console.log('API Response', response);
  } else {
    console.log('HTTP error', xmlhttp.status, xmlhttp.statusText);
  }
};
```

### An example of a GET Ajax Request

```js
var xmlhttp;
if (window.XMLHttpRequest) {
  xmlhttp = new XMLHttpRequest();
} else {
  xmlhttp = new ActiveXObject('Microsoft.XMLHTTP');
}
xmlhttp.open('GET', 'send-ajax-request-url', true);

xmlhttp.setRequestHeader('cache-control', 'no-cache');
xmlhttp.send();

xmlhttp.onreadystatechange = function () {
  if (xmlhttp.readyState !== 4) return;
  if (xmlhttp.status == 200) {
    let response;
    try {
      response = JSON.parse(xmlhttp.responseText);
    } catch (e) {
      response = xmlhttp.responseText;
    }
    console.log('Request Response', response);
  } else {
    console.log('HTTP error', xmlhttp.status, xmlhttp.statusText);
  }
};
```

This article primarily introduces examples to explain the method of using native JavaScript to process AJAX requests, so that friends who require it can refer to this article if the native API is used instead of the Ajax method in jQuery.

