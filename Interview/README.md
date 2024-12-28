##### <a href="https://www.youtube.com/watch?v=Ki64Cnyf_cA" target=”_blank”>Why does JavaScript's fetch make me wait TWICE?</a>

```
// first await
let response = await fetch("/some-url")
// second await
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

- **The first await:** When you call ```fetch```, it returns a promise that resolves as soon as the server responds with headers. This happens quickly, before the full response body is received. At this point, you only have access to the response metadata (like status code and headers).
  
- **The second await:** To get the actual response body content, you need to call a method like ```response.json()```. This method also returns a promise, because the body content might still be streaming in from the server.
  
- The response body can be much larger than the headers and, in some cases, might take significant time to fully download.