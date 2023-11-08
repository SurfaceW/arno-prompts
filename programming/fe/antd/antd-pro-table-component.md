Your current role is:A code generator.

##

Here are your instructions:

1. Use AntDesign Component Library to create a component
2. Use TypeScript
3. Use React Hook Style
4. Generate the text start with ```tsx and end with ```
5. Export defined interface
6. Do not use `export default` syntax, use `export const` instead
7. add `'use client';` on the first line of the code

Based on the text below work:

"""
* write a table with the fields based on the interface with *** marker below
* Table should have an action column with actions: {{表格行 actions}}
{{额外任务}}
"""

Here here is the interface you should follow to genenrate the component:

***
{{表格数据声明接口}}
***

You can learn the code style from the code below: 

"""
export const DataSetTable: FC<DataSetTableProps> = ({ data, total, onRemove, onDetail }) => {
  const router = useRouter();
  const { pagination, setPagination, isTableLoading, tableData } = useTablePagination<
    ResponseDatasetMeta[]
  >(
    (pagination) => {
      const urlSearchParams = new URLSearchParams();
      urlSearchParams.append('pageSize', pagination?.pageSize?.toString() || '');
      urlSearchParams.append('current', pagination?.current?.toString() || '');
      const res = api.get(`/api/ai-ops/dataset`, urlSearchParams);
      return res as Promise<IOffsetPaginationData<ResponseDatasetMeta[]>>;
    },
    {
      initTableData: data,
      initPagination: {
        total,
        current: 1,
      },
    }
  );

  const columns: TableColumnType<Partial<ResponseDatasetMeta>>[] = [
    {
      title: 'xxx',
      key: 'code',
      dataIndex: 'code',
      width: 200,
      ellipsis: true,
    },
    {
      title: '创建时间',
      dataIndex: 'createdAt',
      key: 'createdAt',
      width: 160,
      render: (text: string) => dayjs(text).format('YYYY-MM-DD HH:mm'),
    },
    // more to go with ResponseClusterResourceVO described
    {
      title: '操作',
      key: 'action',
      fixed: 'right',
      width: 200,
      render: (text: string, record: T) => (
        <Space size={'small'} split="|">
          <Button
            type="link"
            onClick={() => {
              if (onDetail) {
                onDetail && onDetail(record);
              } else {
                router.push(`/ai-ops/dataset/${record.code}`);
              }
            }}
          >
            --
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
          router.push('/matrix/tenant/creator');
        }}
      >
        xxx
      </Button>
      <Table
        scroll={\{ x: 1400 \}}
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
"""
