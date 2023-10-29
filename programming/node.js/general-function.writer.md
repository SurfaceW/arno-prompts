You are a Node.js application developer using **Typescript** programming language writing assistant. 

Use Typescript `5.x` to complete the following tasks.

- write a function to help me transform pure digit to human-friendly readable string
- these number are from professional financial report including:
  - some ratios
  - some floating large number
  - some big integer
- you should handle those conditions to output readable format
  
For example:

- input: 1101179000
- output: 11.01B

- input: 22.2231231231
- output: 22

- input: 0.233242342
- output: 23.32%

```typescript
export const prettifyNumber = (number: number): string => {
  // your code here
}
```

* Make sure you have considered all the possible cases.
* Make sure about the type safety & durability.
* If you have question about your implementation, you have to write down your question and your solution in the comment section.
* [Optional] write a unit test for your implementation with `jest` test framework.

---

GPT Output:
```ts
export const prettifyNumber = (number: number): string => {
  // check if the number is a ratio
  if (number < 1 && number > 0) {
    // convert ratio to percentage and round to two decimal places
    return `${(number * 100).toFixed(2)}%`;
  }

  // handle large numbers 
  if (number >= 1000000000) {
    return `${(number / 1000000000).toFixed(2)}B`;
  } else if (number >= 1000000) {
    return `${(number / 1000000).toFixed(2)}M`;
  } else if (number >= 1000) {
    return `${(number / 1000).toFixed(2)}K`;
  }

  // handle floating numbers and integers
  return `${Math.round(number)}`;
}
```