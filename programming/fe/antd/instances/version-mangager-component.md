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
