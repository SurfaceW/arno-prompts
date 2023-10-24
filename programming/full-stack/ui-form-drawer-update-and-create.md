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
* write a drawer component wrap with form fields based ```prisma delimeter below described by **PrismaSchema**
* form fields should include create or update condition
{{Extra Requirements for FORM fields}}
###

Here here is the Entity Prisma Schema you should follow to genenrate the component:
```prisma
model CategoryTag {
  id        BigInt   @id @default(autoincrement())
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
  name      String   @db.VarChar(100)
  desc      String?  @db.VarChar(100)
  type      Int      @default(0)
  status    Int      @default(0)
  createdBy String?  @db.VarChar(40)
  updatedBy String?  @db.VarChar(40)
}
```

Here is the template code you need to use: 

```tsx
'use client';

import { FC, useEffect } from 'react';
import { Drawer, Form, Input, Button, message } from 'antd';
import { api } from '@ailabs/client-share';
import { Demo } from '@ailabs/db';
import { useRouter } from "next/navigation";

interface DemoCreateOrUpdateFormDrawerProps {
  visible: boolean;
  mode: 'edit' | 'new';
  value: Partial<Demo>;
  onClose: () => void;
  onSubmit?: (data: Partial<Demo>) => void;
}

export const DemoCreateOrUpdateFormDrawer: FC<DemoCreateOrUpdateFormDrawerProps> = ({
  visible,
  onClose,
  onSubmit,
  mode,
  value,
}) => {
  const [form] = Form.useForm();
  const [loading, setLoading]  = useState(false);
  const router = useRouter();
  useEffect(() => {
    if (value) {
      form.setFieldsValue({
        ...value,
      });
    }
  }, [value, form]);

  const handleFinish = async (values: any) => {
    onSubmit && onSubmit({
      name: values.name ?? undefined,
    });
    setLoading(true);
    if (mode === 'new') {
      await api
        .put<Partial<CategoryTag>>('/api/path/to/demo', {
          // necessary fileds to create
        })
        .then(() => {
          form.resetFields();
          onClose();
        })
        .catch(() => {
          message.error('创建失败');
        });
      setLoading(false);
    } else {
      await api
        .post<Partial<CategoryTag>>('/api/path/to/demo/demoId', {
          // necessary fields to update
        })
        .then(() => {
          form.resetFields();
          onClose();
        })
        .catch(() => {
          message.error('更新失败');
        });
      setLoading(false);
    }
    router.refresh();
  };

  return (
    <Drawer title="Demo 管理" open={visible} onClose={onClose}>
      <Form form={form} layout="vertical" onFinish={handleFinish}>
        <Form.Item
          label="Demo Field Name"
          name="name"
          rules={[{ required: true, message: '请输入 Demo FieldName' }]}
        >
          <Input />
        </Form.Item>
        // other necessary fields form input here use proper type of Input to implement
        // according to the specific type of dataField
        <Form.Item>
          <Button type="primary" htmlType="submit">
            确认
          </Button>
        </Form.Item>
      </Form>
    </Drawer>
  );
};
```

Here are the extra tasks you may follow:

{{ExtraTasks}}

<TSX Code Generated Only>
