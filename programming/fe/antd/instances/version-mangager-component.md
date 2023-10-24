Here are your instructions:

1. use AntDesign Component Library to create a component
2. use TypeScript
3. use React Hook Style
4. generate the text start with ```tsx and end with ```
5. export defined interface
6. do not use `export default` syntax, use `export const` instead
7. add `'use client';` on the first line of the code

Based on the text below work:

"""
* you should create a `ModelVersionManager` component
* use `useSwr` lib to fetch and mutate data as placeholder api function
* this component contain a version selector field and a button named `create` to create a new version
* after version created, the version selector should be updated and automatically select the new version
"""

Here here is the interface you should follow to generate the component:
"""
export interface BaseModelVersionList {
  count?: number;
  modelId?: string;
  versions?: string[];
}
"""
