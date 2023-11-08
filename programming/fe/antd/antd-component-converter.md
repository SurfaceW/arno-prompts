Here are your instructions:

1. use AntDesign Component Library to create a component
2. use TypeScript
3. use React Hook Style
4. generate the text start with ```tsx and end with ```
5. export defined interface
6. do not use `export default` syntax, use `export const` instead
7. add `'use client';` on the first line of the code
8. use the standard `fetch` API if you need

Based on the tasks below work:
"""
* Convert the card grid into Table implementation
"""

Here is the code you have to transform from:
```tsx
'use client';

import { EditOutlined } from '@ant-design/icons';
import { Card, Col, Empty, Row, Typography } from 'antd';
import { useRouter } from 'next/navigation';
import React, { useEffect, useState } from 'react';

import { UserDingTalkIcon } from '../../common/user-info.client';

type AIModelManifest = {
  id: bigint
  opsId: string
  code: string
  status: number
  createdAt: Date
  updatedAt: Date
  tenantId: string
  name: string
  desc: string | null
  version: string
  manifest: Prisma.JsonValue | null
  createdBy: string | null
  updatedBy: string | null
}


const { Title, Paragraph } = Typography;

export const ModelManifestGrid: React.FC<{ manifestList: AIModelManifest[] }> = ({ manifestList }) => {
  useEffect(() => {
    document.title = 'AIStudio - 基础模型';
  }, []);

  const router = useRouter();
  return (
    <div>
      <Title level={5} className="mb-4">基础模型（BaseModel）</Title>
      <Row gutter={[16, 16]} className="mt-6">
        {manifestList?.length === 0 ? (
          <Empty className="w-full" description="还没有创建基础模型，快来创建一个吧 🤖" />
        ) : (
          ''
        )}
        {manifestList.map((data, index) => (
          <Col key={index} xs={24} sm={12} md={8} lg={8} xl={6} xxl={4}>
            <div
              className="card-container"
              onClick={() => {
                router.push(`/ai-ops/base-model/${data.id}`);
              }}
            >
              <Card
                title={
                  <div className="h-12 flex items-center justify-between">
                    <span>{data.name}</span>
                    <UserDingTalkIcon
                      user={(data as any)?.user}
                    />
                  </div>
                }
                size="small"
                bodyStyle={{ height: '100px', padding: '12px' }}
                style={{ height: '100%' }}
                hoverable
                actions={[
                  <span
                    key="edit"
                    onClick={(e) => {
                      e.stopPropagation();
                      router.push(`/ai-ops/base-model/${data.id}`);
                    }}
                  >
                    编辑 <EditOutlined title="编辑" />
                  </span>,
                ]}
              >
                <Paragraph ellipsis={{ rows: 2 }}>{data.desc}</Paragraph>
              </Card>
            </div>
          </Col>
        ))}
      </Row>
    </div>
  );
};
```
