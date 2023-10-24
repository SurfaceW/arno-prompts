# Example 1

You are a Typescript Assistant.

```ts

type ReplaceUserOptions<T extends Record<string, any> = {}, NEWKEY extends string = string> = {
  /**
   * 需要替换的字段
   */
  targetKey: keyof T;
  /**
   * 使用新的 key
   */
  newKey?: NEWKEY;
  /**
   * 保留的字段
   */
  reserveKeys: Array<keyof User>;
};
  async replaceUserIdByUserFromList<T extends Record<string, any> = {}, NEW_KEY extends string = 'user'>(
    sourceList: Array<T>,
    options: ReplaceUserOptions
  ): Promise<
    T &
      Record<
        ReplaceUserOptions<T>['newKey'] extends string ? ReplaceUserOptions<T, NEW_KEY>['newKey'] : ReplaceUserOptions<T>['targetKey'],
        Record<ReplaceUserOptions['reserveKeys'][number], string | number>
      >
  > {
    const { targetKey, reserveKeys, newKey } = options;
    const bucIdList = sourceList.map((i) => i[targetKey]).filter(Boolean);
    const userList = await this.batchGetUsers(bucIdList);
    return sourceList.map((i) => {
      const user = userList.find((j) => j?.userId?.toString() === i[targetKey]?.toString());
      return {
        ...i,
        [newKey || targetKey]: pick(user, reserveKeys || ['lastName']),
      };
    }) as any;
  }
```

Try to optimize the function's type and make it in a correct way.

# Example 2

Suppose you are a Typescript Typing Writing Assistant.

look at this function written by user:

```ts
const parsedBaseModelList = await userService.replaceUserIdByUserFromList(modelListResponse.modelMeta || [], {
  newKey: 'user',
  targetKey: 'owner',
  reserveKeys: ['loginName', 'lastName', 'empId', 'id'],
});
```

I want you to write a type computation for the method `replaceUserIdByUserFromList` which will generate a new key for the object passed in, use the targetKey to fetch user's information specified as `reservedKeys` fields.

Can you help me write such type?

