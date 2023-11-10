# Antd Pro Table Component Generator

- author: Arno
- date: 2023-11-10
- version: 0.0.1
- models: gpt-4

## Description

Antd Complex table generator with following features:

* support pagination
* support use `swr` to control the data fetching
* support add more features related logic if use more context gpt models


## Prompt

Here are your instructions:

1. use AntDesign Library to create a component
2. use `tailwindcss` to handle custom style if needed
3. Use TypeScript version 5
4. use React Hook Style
5. generate the text start with ```tsx and end with ```
6. export defined interface if needed
7. do not use `export default` syntax, use `export const` instead
8. add `'use client';` on the first line of the code
9. use `swr` library to handle the data fetching and mutation
10. **do output code without explanation**

Based on the text below work:

"""
* you generate a table to show uploaded files
* write a table with the fields based on the interface defined below
* the request api is `POST /api/common/upload/info` use `api.post()` to fetch the data without pagination once fetch all data
* its read-only table
* [optional] table should have an action column with actions: {{Action Descriptor List}}

{{Extra Task Description}}

"""

Here is the interface you should follow to generate the component:

```ts
interface IResponseFileListData {
  code: number;
  reason: string;
  message: string;
  data: Data;
  requestId: string;
}

interface Data {
  files: File[];
}

interface File {
  fileId: string;
  downloadLink: string;
  meta: Meta;
}

interface Meta {
  etag: string;
  lastModified: string;
  size: string;
  type: string;
}
{{Table Row Data Interface}}
```


You can learn the code style from the code below: 

"""
{{Code Example}}
"""

## Examples and Instances

### Prompt Example output

with GPT-4

```tsx
'use client';

import { Table } from 'antd';
import { ColumnProps } from 'antd/es/table';
import { useSWR } from 'swr';
import { api } from 'api/api';
import { FC } from 'react';

interface IResponseFileListData {
  code: number;
  reason: string;
  message: string;
  data: Data;
  requestId: string;
}

interface Data {
  files: File[];
}

interface File {
  fileId: string;
  downloadLink: string;
  meta: Meta;
}

interface Meta {
  etag: string;
  lastModified: string;
  size: string;
  type: string;
}

export const FileTable: FC = () => {
  const { data, error } = useSWR<IResponseFileListData>('/api/common/upload/info', api.post);

  const columns: ColumnProps<File>[] = [
    { title: 'File Id', dataIndex: 'fileId', key: 'fileId' },
    { title: 'Download Link', dataIndex: 'downloadLink', key: 'downloadLink' },
    { title: 'Etag', dataIndex: ['meta', 'etag'], key: 'etag' },
    { title: 'Last Modified', dataIndex: ['meta', 'lastModified'], key: 'lastModified' },
    { title: 'Size', dataIndex: ['meta', 'size'], key: 'size' },
    { title: 'Type', dataIndex: ['meta', 'type'], key: 'type' },
  ];

  if (error) return <div>Error loading data</div>;

  return (
    <Table<File>
      className="w-full"
      loading={!data}
      dataSource={data?.data.files}
      columns={columns}
      rowKey="fileId"
    />
  );
};
```
