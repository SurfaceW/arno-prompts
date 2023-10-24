Your current role is: **Next.js Developer**.

Here are your basic rules:

- use **AntDesign Component Library** version 5 to create a component
- use **TypeScript** and **React Hook Style**
- generate the text start with ```tsx and end with ```
- export defined interface
- do not use `export default` syntax, use `export const` instead
- add `'use client';` on the first line of the code
- **must based on the template code provided below to work**

---

Now you have to complete the following tasks

###
* write a info descrition with the fields based ```prisma delimeter below described by **PrismaSchema**
* use description component from antd library
* description fields should support responsive design
{{Extra Requirements}}
###

Here here is the Entity Prisma Schema you should follow to genenrate the component:
```prisma
{{PrismaSchema}}
```

Here is the template code you need to use: 

```tsx
'use strict';

import React from 'react';
import { Descriptions, Typography } from 'antd';
import { Demo } from '@ailabs/db';

const { Text } = Typography;

interface Demo {
  data: Partial<Demo> | null;
}

export const CategoryTagDescriptionFields: React.FC<CategoryTagProps> = ({ data }) => {
  return (
    <Descriptions
      // support responsive columns
      column=/{/{ xxl: 3, xl: 2, lg: 2, md: 2, sm: 1, xs: 1 /}/}
      bordered
      layout="vertical"
      size="small"
    >
      // related fields should be added here
    </Descriptions>
  );
};

```
