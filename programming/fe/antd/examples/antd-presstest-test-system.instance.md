* bot: https://poe.com/antdplusnextjs
* chat-instance: https://poe.com/chat/2t9z0cpsqdhb9ht6uff
* template ref -> `antd-component-bot.md`

# Create PressureTest Form

According to the given information below:

```ts
/**
 * @description 创建压测任务
 *
 * @tags admin管理接口
 * @name MaasCodeBenchmarkCreate
 * @summary 创建压测任务
 * @request POST:/admin/maas/:code/benchmark
 */
maasCodeBenchmarkCreate: (code: string, request: RequestBenchmarkAddRequest, params: RequestParams = {}) =>
  this.request<
    any,
    ApiCommonResponse & {
      data?: ApiCodeResponse;
    }
  >({
    path: `/admin/maas/${code}/benchmark`,
    method: "POST",
    body: request,
    type: ContentType.Json,
    ...params,
  }),

export interface RequestBenchmarkAddRequest {
  /** 必填，客户端数 */
  clientNum?: string;
  /** 必填，code */
  code?: string;
  /** 必填，并发数 */
  concurrencyNum?: string;
  /** 必填，压测数据文件名 */
  data?: string;
  /** 必填，持续时间 */
  durationTime?: string;
  /** 必填，请求方法 */
  method?: string;
  /** 必填，请求路径 */
  path?: string;
}
```

generate a form to post data use `useMutation` and pick the form fields that meet the payload data requirement.

# MaaS Pressure Test Table

Generate a table to display the pressure test result with the following interface:
* the related APIs and interfaces are described below:

```ts
/**
 * @description 列举压测任务
 *
 * @tags admin管理接口
 * @name MaasCodeBenchmarkList
 * @summary 列举压测任务
 * @request GET:/admin/maas/:code/benchmark
 */
maasCodeBenchmarkList: (
  code: string,
  query: {
    /** 一页的数量, e.g 10，不能超过1000 */
    limit: number;
    /** 页码, e.g 1 */
    current: string;
  },
  params: RequestParams = {},
) =>
  this.request<
    any,
    ApiCommonResponse & {
      data?: ResponseBenchmarkListResponse;
    }
  >({
    path: `/admin/maas/${code}/benchmark`,
    method: "GET",
    query: query,
    type: ContentType.Json,
    ...params,
  }),


export interface ResponseBenchmarkListResponse {
  benchmarks?: ResponseBenchmark[];
  count?: number;
}

export interface ResponseBenchmark {
  clientNum?: number;
  code?: string;
  concurrencyNum?: number;
  data?: string;
  durationTime?: number;
  endTime?: string;
  id?: number;
  maasCode?: string;
  method?: string;
  path?: string;
  report?: string;
  startTime?: string;
  status?: number;
}

```

Table should have the following actions column, which should be rendered with `Space` component wrap `Button` component:

* when user click the `start` button, call the `maasCodeStartBenchmarkCreate` API to start the `MaaS Pressure Test`
* when user click the `stop` button, call the `maasCodeStopBenchmarkCreate` API to stop the `MaaS Pressure Test`
* when user click the `report` button, call the `maasCodeBenchmarkReport` API to get the `MaaS Pressure Test` report as a new component, open the drawer and use `Descriptions` to display the report fields

