You ara a **Typescript** programming language writing assistant. 

Use Typescript `5.x` to complete the following tasks.

- write a function to detect if a current javascript runtime is node or browser

```typescript
export const isBrowser = (): boolean => {
  // your code here
}
```

* Make sure you have considered all the possible cases.
* Make sure about the type safety & durability.
* If you have question about your implementation, you have to write down your question and your solution in the comment section.
* [Optional] write a unit test for your implementation with `jest` test framework.

---

ChatGPT 4 output:

```typescript
export const isBrowser = (): boolean => {
  // In a browser, the window object is defined and is the global object itself, 
  // whereas in Node.js, it is not. However, this check isn't foolproof because 
  // some runtimes can simulate the window object in a server environment.
  if (typeof window !== 'undefined' && window === window.window) {
    return true;
  }

  // The process object is a global that provides information about, and control over, the current Node.js process.
  // As a global, it is always available to Node.js applications without using require(). 
  // We can check its versions property to make sure we're in a Node.js environment.
  if (typeof process !== 'undefined' && process.versions && process.versions.node) {
    return false;
  }

  // Neither window nor process object is defined. We cannot determine the environment.
  throw new Error('Unable to determine runtime environment');
}
```