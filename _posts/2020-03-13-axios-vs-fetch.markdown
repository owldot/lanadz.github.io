---
layout: post
title: 'Axios or fetch(): Which should you use?'
date: 2020-03-13
tags:
  - javascript
keywords:
  - axios or fetch
  - request with js
  - axios vs fetch
dont_show_excerpt_separator: false
---

Original source: [link](https://blog.logrocket.com/axios-or-fetch-api/){:target="\_blank"}

<!--more-->

Without question, some developers prefer Axios over built-in APIs for its ease of use. But many overestimate the need for such a library. The `fetch()` API is perfectly capable of reproducing the key features of Axios, and it has the added advantage of being readily available in all modern browsers.

In this article, we will compare `fetch()` and `Axios` and see how they can be used to perform different tasks. Hopefully, by the end of the article, you’ll have a better understanding of both APIs.
Basic syntax

Before we delve into more advanced features of Axios, let’s compare its basic syntax to `fetch()`. Here’s how you can use Axios to send a POST request with custom headers to a URL. Axios automatically converts the data to JSON, so you don’t have to.

```javascript
// axios

const options = {
  url: 'http://localhost/test.htm',
  method: 'POST',
  headers: {
    Accept: 'application/json',
    'Content-Type': 'application/json;charset=UTF-8'
  },
  data: {
    a: 10,
    b: 20
  }
};

axios(options).then((response) => {
  console.log(response.status);
});
```

Now compare this code to the fetch() version, which produces the same result:

```javascript
// fetch()

const url = 'http://localhost/test.htm';
const options = {
  method: 'POST',
  headers: {
    Accept: 'application/json',
    'Content-Type': 'application/json;charset=UTF-8'
  },
  body: JSON.stringify({
    a: 10,
    b: 20
  })
};

fetch(url, options).then((response) => {
  console.log(response.status);
});
```

Notice that:

> To send data, `fetch()` uses the `body` property, while Axios uses the `data` property
> The `data` in `fetch()` is stringified
> The URL is passed as an argument to `fetch()`. In Axios, however, the URL is set in the options object

### Backward compatibility

One of the main selling points of Axios is its wide browser support. Even old browsers like IE11 can run Axios without any issue. Fetch(), on the other hand, only supports Chrome 42+, Firefox 39+, Edge 14+, and Safari 10.1+

If your only reason for using Axios is backward compatibility, you don’t really need an HTTP library. Instead, you can use `fetch()` with a polyfill like this to implement similar functionality on web browsers that do not support fetch(). To begin using the fetch polyfill, install it via `npm` command:

`npm install whatwg-fetch --save`

Then you can make requests like this:

```javascript
import 'whatwg-fetch'
window.fetch(...)
```

Keep in mind that that you might also need a promise polyfill in some old browsers.

### Response timeout

The simplicity of setting timeout in Axios is one of the reasons some developers prefer it to `fetch()`. In Axios, you can use the optional timeout property in the config object to set the number of milliseconds before the request is aborted. For example:

```javascript
axios({
  method: 'post',
  url: '/login',
  timeout: 4000, // 4 seconds timeout
  data: {
    firstName: 'David',
    lastName: 'Pollock'
  }
})
  .then((response) => {
    /_ handle the response _/;
  })
  .catch((error) => console.error('timeout exceeded'));
```

Fetch() provides similar functionality through the `AbortController` interface. It’s not as simple as the Axios version, though:

```javascript
const controller = new AbortController();
const options = {
  method: 'POST',
  signal: controller.signal,
  body: JSON.stringify({
    firstName: 'David',
    lastName: 'Pollock'
  })
};
const promise = fetch('/login', options);
const timeoutId = setTimeout(() => controller.abort(), 4000);

promise
  .then((response) => {
    /_ handle the response _/;
  })
  .catch((error) => console.error('timeout exceeded'));
```

Here, we create an `AbortController` object using the AbortController. AbortController() constructor, which allows us to later abort the request. signal is a read-only property of AbortController providing a means to communicate with a request or abort it. If the server doesn’t respond in less than four seconds, controller.abort() is called, and the operation is terminated.

### Automatic JSON data transformation

As we said earlier, Axios automatically stringifies the data when sending requests (though you can override the default behavior and define a different transformation mechanism). When using `fetch()`, however, you’d have to do it manually. Compare:

```javascript
// axios
axios.get('https://api.github.com/orgs/axios').then(
  (response) => {
    console.log(response.data);
  },
  (error) => {
    console.log(error);
  }
);

// fetch()
fetch('https://api.github.com/orgs/axios')
  .then((response) => response.json()) // one extra step
  .then((data) => {
    console.log(data);
  })
  .catch((error) => console.error(error));
```

Automatic transformation of data is a nice feature to have, but again, it’s not something you can’t do with fetch().

### HTTP interceptors

One of the key features of Axios is its ability to intercept HTTP requests. HTTP interceptors come in handy when you need to examine or change HTTP requests from your application to the server or vice versa (e.g., logging, authentication, etc.). With interceptors, you won’t have to write separate code for each HTTP request.

Here’s how you can declare a request interceptor in Axios:

```javascript
axios.interceptors.request.use((config) => {
  // log a message before any HTTP request is sent
  console.log('Request was sent');

  return config;
});

// sent a GET request
axios.get('https://api.github.com/users/sideshowbarker').then((response) => {
  console.log(response.data);
});
```

In this code, the `axios.interceptors.request.use()` method is used to define a code to be run before an HTTP request is sent.

By default, `fetch()` doesn’t provide a way to intercept requests, but it’s not hard to come up with a workaround. You can overwrite the global `fetch` method and define your own interceptor, like this:

```javascript
fetch = ((originalFetch) => {
  return (...arguments) => {
    const result = originalFetch.apply(this, arguments);
    return result.then(console.log('Request was sent'));
  };
})(fetch);

fetch('https://api.github.com/orgs/axios')
  .then((response) => response.json())
  .then((data) => {
    console.log(data);
  });
```

### Download progress

Progress indicators are very useful when loading large assets, especially for users with slow internet speed. Previously, JavaScript programmers used the `XMLHttpRequest.onprogress` callback handler to implement progress indicators.

The Fetch API doesn’t have an onprogress handler. Instead, it provides an instance of `ReadableStream` via the `body` property of the response object.

The following example illustrates the use of ReadableStream to provide users with immediate feedback during image download:

```html
// original code: https://github.com/AnthumChris/fetch-progress-indicators

<div id="progress" src="">progress</div>
<img id="img" />

<script>
  'use strict';

  const element = document.getElementById('progress');

  fetch('https://fetch-progress.anthum.com/30kbps/images/sunrise-baseline.jpg')
    .then((response) => {
      if (!response.ok) {
        throw Error(response.status + ' ' + response.statusText);
      }

      // ensure ReadableStream is supported
      if (!response.body) {
        throw Error('ReadableStream not yet supported in this browser.');
      }

      // store the size of the entity-body, in bytes
      const contentLength = response.headers.get('content-length');

      // ensure contentLength is available
      if (!contentLength) {
        throw Error('Content-Length response header unavailable');
      }

      // parse the integer into a base-10 number
      const total = parseInt(contentLength, 10);

      let loaded = 0;

      return new Response(
        // create and return a readable stream
        new ReadableStream({
          start(controller) {
            const reader = response.body.getReader();

            read();
            function read() {
              reader
                .read()
                .then(({ done, value }) => {
                  if (done) {
                    controller.close();
                    return;
                  }
                  loaded += value.byteLength;
                  progress({ loaded, total });
                  controller.enqueue(value);
                  read();
                })
                .catch((error) => {
                  console.error(error);
                  controller.error(error);
                });
            }
          }
        })
      );
    })
    .then((response) =>
      // construct a blob from the data
      response.blob()
    )
    .then((data) => {
      // insert the downloaded image into the page
      document.getElementById('img').src = URL.createObjectURL(data);
    })
    .catch((error) => {
      console.error(error);
    });

  function progress({ loaded, total }) {
    element.innerHTML = Math.round((loaded / total) * 100) + '%';
  }
</script>
```

Implementing a progress indicator in Axios is simpler, especially if you use the Axios Progress Bar module. First, you need to include the following style and script:

```html
<link
  rel="stylesheet"
  type="text/css"
  href="https://cdn.rawgit.com/rikmms/progress-bar-4-axios/0a3acf92/dist/nprogress.css"
/>

<script src="https://cdn.rawgit.com/rikmms/progress-bar-4-axios/0a3acf92/dist/index.js"></script>
```

Then you can implement the progress bar like this:

```html
<img id="img" />

<script>
  loadProgressBar();

  const url =
    'https://fetch-progress.anthum.com/30kbps/images/sunrise-baseline.jpg';

  function downloadFile(url) {
    axios
      .get(url, { responseType: 'blob' })
      .then((response) => {
        const reader = new window.FileReader();
        reader.readAsDataURL(response.data);
        reader.onload = () => {
          document.getElementById('img').setAttribute('src', reader.result);
        };
      })
      .catch((error) => {
        console.log(error);
      });
  }

  downloadFile(url);
</script>
```

This code uses the `FileReader` API to asynchronously read the downloaded image. The `readAsDataURL` method returns the image's data as a Base64-encoded string, which is then inserted into the `src` attribute of the `img` tag to display the image.

### Simultaneous requests

To make multiple simultaneous requests, Axios provides the `axios.all()` method. Simply pass an array of requests to this method, then use axios.spread() to assign the properties of the response array to separate variables:

```javascript
axios
  .all([
    axios.get('https://api.github.com/users/iliakan'),
    axios.get('https://api.github.com/users/taylorotwell')
  ])
  .then(
    axios.spread((obj1, obj2) => {
      // Both requests are now complete
      console.log(
        obj1.data.login +
          ' has ' +
          obj1.data.public_repos +
          ' public repos on GitHub'
      );
      console.log(
        obj2.data.login +
          ' has ' +
          obj2.data.public_repos +
          ' public repos on GitHub'
      );
    })
  );
```

You can achieve the same result by using the built-in `Promise.all()` method. Pass all fetch requests as an array to `Promise.all()`. Next, handle the response by using an async function, like this:

```javascript
Promise.all([
  fetch('https://api.github.com/users/iliakan'),
  fetch('https://api.github.com/users/taylorotwell')
])
  .then(async ([res1, res2]) => {
    const a = await res1.json();
    const b = await res2.json();
    console.log(a.login + ' has ' + a.public_repos + ' public repos on GitHub');
    console.log(b.login + ' has ' + b.public_repos + ' public repos on GitHub');
  })
  .catch((error) => {
    console.log(error);
  });
```

### Conclusion

Axios provides an easy-to-use API in a compact package for most of your HTTP communication needs. However, if you prefer to stick with native APIs, nothing stops you from implementing Axios features.

As discussed in this article, it’s perfectly possible to reproduce the key features of the Axios library using the fetch() method provided by web browsers. Ultimately, whether it’s worth loading a client HTTP API depends on whether you’re comfortable working with built-in APIs.

Original article: [here](https://blog.logrocket.com/axios-or-fetch-api/){:target="\_blank"}
