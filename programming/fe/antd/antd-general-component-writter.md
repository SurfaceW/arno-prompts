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

More business related context are given below:

"""
{{Your Business Context}}
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

### Pipeline Guide

2023-11-08

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
* try to use `Typography` component to make it pretty, `url` use `Typography.Link`
"""

Here here is the interface you should follow to generate the component:


```ts
export interface IPipeLineDetailResponse {
  code: string;
  status: number;
  template: IPipelineTemplate[];
}

export interface IPipelineTemplate {
  name: string;
  currentStepCode: string;
  steps: IPipelineStep[];
}

export interface IPipelineStep {
  code: string;
  name: string;
  url: string;
  isInnerUrl: boolean;
  desc: string;
}
```

<TSXCode>
```

### Complex Logic Guide

2023-11-08


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
* write a **high-order-component** to wrap a sub-component to do the operation or oa flow check approval logic
* this component is not directly concerned with View rendering, it share the logic of check oa or operation flow approval
* this component use the API described by the interface below to fetch the approval flow information
* the operation steps are described here:
  1. when user click on ChildComponent, it will trigger the `onCheckApproval` function passed by this high-order component as props
  2. High-order component will fetch the approval flow information from remote api use `swr` impl, and pass `isChecking` as loading indicator to ChildComponent
  3. if approve flow is not exist, it will popup a modal to ask user to create a new approval flow, if user choose to do so, open a new tab and jump to the `/ai-ops/pipeline` page, and pass the `module` and `type` as query params, and pass the `code` as query params
  4. if approve flow is exist, it will check the approval status, if status is `ChangeStatusPending`, it will open a modal with message to user to wait for approval, the modal should contain a link to that approval page, and pass approval result to its ChildComponent
  5. if status is `ChangeStatusPass` it will pass approvalResult to ChildComponent
* because api client is auto-generated by swagger client, you don't need to implement the api client, just use the useSwr hook and `api.[HTTPMethodName]` to placeholder and mock the request with apiPathName `/api/ai-ops/oa`, and I shall finish them later
"""

Here are the related API design you should know to encapsulate the logic:

```business and api described
changeStatus (
	ChangeStatusPending ChangeStatus = 0      // 变更中
	ChangeStatusPass    ChangeStatus = 1      // 审批完成
	ChangeStatusDone    ChangeStatus = 2      // 变更完成
)
---
查询变更状态
GET /oa/tenants/:tenantId/owners/:ownerId/change/status
参数名 说明
module 操作模块
type 操作类型
code 资源唯一标识
Response
{
	"code": 0,
	"reason": "",
	"message": "",
	"data": {
		"id": "",
		"code": "",
        "module": "",
        "type": "",
        "resourceCode": ""
        "status": "", 
		"owner": ""
	},
	"requestId": ""
}
---
创建变更
POST /model/tenants/:tenantId/owners/:ownerId/change
{
    "module": "",
    "type": "",
    "resourceCode": ""
}
{
    "code": 0,
    "reason": "",
    "message": "",
      "data": {
    "code": ""
    },
    "requestId": ""
}
```

Here here is the interface I want to use to describe the component props:


```ts
export interface IOAFlowApprovalProps {
  module: string;
  type: string;
  code: string;
}

export interface IOAFlowApprovalPassToChildComponentProps {
  // props used to indicate the component is checking oa or not
  isChecking: boolean;
  // props call onCheckApproval to trigger the oa check logic used by ChildComponent
  onCheckApproval: (approval: IApproval) => Promise<{}>;
  // IApprovalResult is the result of the approval check described by the API response above
  approvalResult: IApprovalResult;
}
```

<TSXCode>
```

#### GPT Generated Code

```tsx
'use client';
import React, { useState, useCallback } from 'react';
import { Modal } from 'antd';
import useSWR from 'swr';
import { Button } from 'antd';

export interface ChangeStatus {
  status: 'ChangeStatusPending' | 'ChangeStatusPass' | 'ChangeStatusDone';
}

export interface IApprovalResult {
  id: string;
  code: string;
  module: string;
  type: string;
  resourceCode: string;
  status: ChangeStatus;
  owner: string;
}

export interface IOAFlowApprovalProps {
  module: string;
  type: string;
  code: string;
  tenantId: string;
  ownerId: string;
}

export interface IOAFlowApprovalPassToChildComponentProps {
  isChecking: boolean;
  onCheckApproval: (approval: IApprovalResult) => Promise<{}>;
  approvalResult?: IApprovalResult;
}

const withOAFlowApproval = <P extends object>(ChildComponent: React.ComponentType<P & IOAFlowApprovalPassToChildComponentProps>) =>
  ({ module, type, code, tenantId, ownerId }: IOAFlowApprovalProps) => {
    const [isChecking, setIsChecking] = useState(false);
    const { data: approvalData, error } = useSWR<IApprovalResult>(`/api/ai-ops/oa/tenants/${tenantId}/owners/${ownerId}/change/status?module=${module}&type=${type}&code=${code}`);

    const handleCheckApproval = useCallback(() => {
      setIsChecking(true);
      // Placeholder for API call, actual API integration will be added later
      // Assuming 'api.GET' is the auto-generated API client method
      const approval = await api.GET(`/oa/tenants/${tenantId}/owners/${ownerId}/change/status?module=${module}&type=${type}&code=${code}`);
      setIsChecking(false);

      if (approval.status === 'ChangeStatusPending') {
        Modal.info({
          title: 'Approval Pending',
          content: `Please wait for approval. You can check the status here: <approval-link>.`,
        });
        return approval;
      } else if (approval.status === 'ChangeStatusPass') {
        return approval;
      } else {
        Modal.confirm({
          title: 'Create Approval Flow',
          content: 'No approval flow exists. Would you like to create one?',
          onOk() {
            window.open(`/ai-ops/pipeline?module=${module}&type=${type}&code=${code}`, '_blank');
          },
        });
      }
    }, [module, type, code, tenantId, ownerId]);

    if (error) {
      console.error('Error fetching approval data', error);
    }

    return (
      <ChildComponent
        {...(props as P)}
        isChecking={isChecking}
        onCheckApproval={handleCheckApproval}
        approvalResult={approvalData}
      />
    );
  };

// Export interface so it can be used elsewhere
export { IOAFlowApprovalProps, IOAFlowApprovalPassToChildComponentProps };

// Example usage of high-order-component with a button as a child component
export const ApprovalButton = withOAFlowApproval(Button);

// Note: This is a simplified example. In a real-world scenario, the HOC should handle different API states including loading and error.
```