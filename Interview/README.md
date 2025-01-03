#### <a href="https://www.youtube.com/watch?v=Ki64Cnyf_cA" target="_blank">Q#01 Why does JavaScript's fetch make me wait TWICE?</a>

```
// First await: Wait for headers
let response = await fetch("/some-url")

// Second await: Wait for the body and parse as JSON
let myObject = await response.json()
```

```

let response = await fetch("/some-url")
// At this point,
// 1. the client has received the headers
// 2. the body is (probably) still making its way over.

let myObject = await response.json()
// At this point,
// 1. the client has received the body
// 2. the client has tried to parse the body
// as json with the goal of making a JavaScript object
```

- **The first await:** When you call ```fetch```, it returns a promise that resolves as soon as the server responds with headers. This happens quickly, before the full response body is received. At this point, the client has received the **headers**, the **body** may still be in transit, and you only have access to the response **metadata** (like status code and headers).
  
- **The second await:** To get the actual response body content, you need to call a method like ```response.json()```. This method also returns a promise, because the body content might still be streaming in from the server.
  
- The response body can be much larger than the headers and, in some cases, might take significant time to fully download.


#### Q#02 most common most common use cases for memory leak 

```Global Variables without using let, const or var```
- Variables without let, const or var will automatically be part of a global Scope and can corrupt the global scope.

```Not removing Event Listeners once their purpose is done```
- it's important to remove event listeners once the listen to the event is completed otherwise they will listen for infinite time until web pages available.

```Mindfully use the Closures```
- Closures can retain the references for longer duration can cause memory leak 

```Not cleared intervals ```
- Uncleared setInterval() keep associated object in memory.