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
* write a table with the fields based ```prisma delimeter below described by **PrismaSchema**
* table should use the template code below to generate
###

Here here is the Entity Prisma Schema you should follow to genenrate the component:
```prisma
{{PrismaSchema}}
```

Here is the template code you need to use: 

```tsx
'use client';

import { Button, message, Modal, Space, Table, TableColumnType, Drawer } from "antd";
import dayjs from "dayjs";
import { useRouter } from "next/navigation";
import React, { FC } from "react";
import { setTimeout } from "timers";

import { useTablePagination } from "@/components/client/hooks/use-table";
import { api, IOffsetPaginationData } from "@ailabs/client-share";
import { Demo } from "@ailabs/db";

import { DemoCreateOrUpdateFormDrawer } from "./tag-add-and-update.drawer.client";
import { DemoDescriptionFields } from "./tag-detail.drawer.client";

interface DemoTableProps {
  data: Demo[];
  total: number;
  onAdd?: (meta: Partial<Demo>) => void;
  onRemove?: (meta: Partial<Demo>) => void;
  onDetail?: (meta: Partial<Demo>) => void;
}

export const DemoTable: FC<DemoTableProps> = ({ data, total, onRemove, onDetail }) => {
  const router = useRouter();
  const [showFormDrawer, setShowFormDrawer] = React.useState(false);
  const [formDrawerType, setFormDrawerType] = React.useState<'edit' | 'new'>('new');
  const [showDetailDrawer, setShowDetailDrawer] = React.useState(false);
  const [currentSelectedRow, setCurrentSelectedRow] = React.useState<Partial<CategoryTag> | null>(null);
  const { pagination, setPagination, isTableLoading, tableData } = useTablePagination<
    CategoryTag[]
  >(
    (pagination) => {
      const urlSearchParams = new URLSearchParams();
      urlSearchParams.append('pageSize', pagination?.pageSize?.toString() || '');
      urlSearchParams.append('current', pagination?.current?.toString() || '');
      const res = api.get(`/api/path/to/demo`, urlSearchParams);
      return res as Promise<IOffsetPaginationData<Demo[]>>;
    },
    {
      initTableData: data,
      initPagination: {
        total,
        current: 1,
      },
    }
  );

  const columns: TableColumnType<Partial<Demo>>[] = [
    {
      title: 'ID',
      key: 'id',
      dataIndex: 'id',
      width: 200,
      ellipsis: true,
    },
    {
      title: 'Demo 名称',
      dataIndex: 'name',
      key: 'name',
      width: 200,
      ellipsis: true,
    },
    {
      title: 'Demo 创建者',
      dataIndex: 'createdBy',
      key: 'createdBy',
      width: 100,
      render(owner: any) {
        return owner?.loginName || owner?.lastName || owner;
      },
    },
    {
      title: '创建时间',
      dataIndex: 'createdAt',
      key: 'createdAt',
      width: 160,
      render: (text: string) => dayjs(text).format('YYYY-MM-DD HH:mm'),
    },
    {
      title: '更新时间',
      dataIndex: 'updatedAt',
      key: 'updatedAt',
      width: 160,
      render: (text: string) => dayjs(text).format('YYYY-MM-DD HH:mm'),
    },
    {
      title: '操作',
      key: 'action',
      fixed: 'right',
      width: 200,
      render: (text: string, record: Partial<CategoryTag>) => (
        <Space size={'small'} split="|">
          {/* 待重构为模态框详情（Drawer 设计） */}
          <Button
            type="link"
            onClick={() => {
              if (onDetail) {
                onDetail && onDetail(record);
              } else {
                setCurrentSelectedRow(record);
                setShowDetailDrawer(true);
              }
            }}
          >
            详情
          </Button>
          <Button
            type="link"
            onClick={() => {
              setFormDrawerType('edit');
              setCurrentSelectedRow(record);
              setShowFormDrawer(true);
            }}
          >
            编辑
          </Button>
          <Button
            type="link"
            danger
            onClick={async () => {
              Modal.confirm({
                title: '删除 Demo',
                content: '确定删除该 Demo 吗？',
                onOk: async () => {
                  await api
                    .delete(`/api/path/to/demo/${record.id}`)
                    .catch(() => {
                      message.error('删除失败');
                    })
                    .then(() => {
                      message.success('删除成功');
                      setPagination({
                        ...pagination,
                        current: 1,
                      });
                      setTimeout(() => {
                        location.reload();
                      }, 1000);
                    });
                },
              });
            }}
          >
            删除
          </Button>
        </Space>
      ),
    },
  ];

  return (
    <>
      <Button
        type="primary"
        className="mb-4"
        onClick={() => {
          router.push('/ai-assets/tags');
        }}
      >
        创建新的 Demo 💿
      </Button>
      <DemoCreateOrUpdateFormDrawer
        visible={!!showFormDrawer}
        onClose={() => {
          setShowFormDrawer(false);
        }}
        mode={formDrawerType}
        value={currentSelectedRow}
      />
      <Drawer open={showDetailDrawer} width={800} onClose={() => {
        setShowDetailDrawer(false);
      }}>
        <DemoDescriptionFields data={currentSelectedRow} />
      </Drawer>
      <Table
        scroll=\{\{ x: 1400 \}\}
        loading={isTableLoading}
        size="small"
        pagination={pagination}
        onChange={(pag) => {
          if (pag.current?.toString() !== pagination.current?.toString()) {
            setPagination({
              ...pagination,
              ...pag,
            });
          }
        }}
        dataSource={tableData || []}
        columns={columns}
      />
    </>
  );
};

```

Here are the extra tasks you may follow:

{{ExtraTasks}}

<TSX Code Generated Only>
