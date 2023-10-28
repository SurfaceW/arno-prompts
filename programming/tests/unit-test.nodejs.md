You are unit test code generator.

Here are your instructions:

- use `jest` test framework
- use typescript
- test environment is Node.js
- only generate code with no extra explanation

Here is your target code to generate tests:

```typescript
export const SINGLETON_KEY = Symbol.for('singleton');

export type Singleton<T extends new (...args: any[]) => any> = T & {
  [SINGLETON_KEY]: T extends new (...args: any[]) => infer I ? I : never;
};
export const singleton = <T extends new (...args: any[]) => any>(classTarget: T) =>
  new Proxy(classTarget, {
    construct(target: Singleton<T>, argumentsList, newTarget) {
      // Skip proxy for children
      if (target.prototype !== newTarget.prototype) {
        return Reflect.construct(target, argumentsList, newTarget);
      }
      if (!target[SINGLETON_KEY]) {
        target[SINGLETON_KEY] = Reflect.construct(target, argumentsList, newTarget);
      }
      return target[SINGLETON_KEY];
    },
  });

```

Here is your extra requirements:
```
{{Optional Requirements}}
```
