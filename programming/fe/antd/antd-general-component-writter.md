# AntD Component Generator

- author: Arno
- date: 2023-11-08
- version: 0.0.1
- models: gpt-3.5 / gpt-4

## Description

Generate AntDesign component with TypeScript and React Hook Style.

## Prompt Structure

```md

Here are your instructions:

1. use AntDesign Component Library to create a component
2. use TypeScript
3. use React Hook Style
4. generate the text start with ```tsx and end with ```
5. export defined interface
6. do not use `export default` syntax, use `export const` instead
7. add `'use client';` on the first line of the code
8. use the standard `useSwr` or `useMutation` from swr lib API if you need
9. **do output code without explanation**

Based on the text below work:

"""
{{Your Task}}
"""

Here here is the interface you should follow to generate the component:

"""
{{Your Interface Context}}
"""

You can learn the code style from the code below: 

```tsx
{{Your Reference Code}}
```

<TSXCode>
```


## Examples and Instances

### Version Manager

2023-10-32

```md
Here are your instructions:

1. use AntDesign Component Library to create a component
2. use TypeScript
3. use React Hook Style
4. generate the text start with ```tsx and end with ```
5. export defined interface
6. do not use `export default` syntax, use `export const` instead
7. add `'use client';` on the first line of the code
8. **do output the code without redundant explaining**

Based on the text below work:

"""
* you should create a `ModelVersionManager` component, which expose:
  * `onVersionChange` to handle version change event
  * `currentVersion` as controlled props
* use `useSwr` lib to fetch and mutate data as placeholder api function like `useSwr()` and `useSwrMutation()` from `swr` lib
* this component contain a version selector field and a button named `create` to open a new modal with a input field and make user write a version string to create a new version
* version string should obey the semi-version validation rule
* after version created, the version selector should be updated and automatically select the new version
"""

Here here is the interface you should follow to generate the component:
```ts
export interface BaseModelVersionList {
  count?: number;
  modelId?: string;
  versions?: string[];
}
```

<TSXCode>
```

### Tree Operation Guide

```md
Here are your instructions:

1. use AntDesign Component Library to create a component
2. use TypeScript
3. use React Hook Style
4. generate the text start with ```tsx and end with ```
5. export defined interface
6. do not use `export default` syntax, use `export const` instead
7. add `'use client';` on the first line of the code
8. use the standard `useSwr` or `useMutation` from swr lib API if you need
9. **do output code without explanation**

Based on the text below work:

"""
* write a component for a *pipeline* liked stuff to display pipeline information and url operations
* try to use `Steps` component of AntDesign to implement
* the content of steps can be defined by the interface below, try to follow the interface to generate the display view of the component
* there should be a refresh button on the right-top of the steps, which can use to refresh the steps content manually, after click you can fetch from remote api use `swr` impl.
* only show current step information described by the interface below
"""

Here here is the interface you should follow to generate the component:


```ts
interface ISteps {}
```

<TSXCode>
```

