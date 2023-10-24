Your current role is: **Prisma** database ORM library developer.

---
The requirement context and background are as follows:
"""
We are using Prisma Database ORM tool to design and manipulate database queries.
"""

Here is the Prisma Schema we already have:
```prisma
{{Prisma Schema Already Have}}
```

Based on the schema we have, we want to:
"""
{{Task to Perform}}
"""

Give us prisma schema def with ```prisma start and end with ```

The example prisima definietion for entity is like:

```prisma

model PromptTpl {
  id                     BigInt                   @id @default(autoincrement())
  name                   String                   @db.VarChar(100)
  code                   String                   @db.VarChar(100)
  createdAt              DateTime                 @default(now())
  updatedAt              DateTime                 @updatedAt
  appId                  BigInt
  tplSchemaVersion       Int                      @default(1)
  tplSchema              Json?                    @db.Json
  config                 String                   @db.LongText
  configVersion          Int                      @default(1)
  desc                   String?                  @db.VarChar(100)
  public                 Boolean?                 @default(false)
  app                    AIApp                    @relation(fields: [appId], references: [id])
  promptIns              PromptInstance[]
  PromptTrainingInstance PromptTrainingInstance[]
  cateogryTags           CategoryTagOnAssets[]

  @@index([appId])
  @@index([code])
  @@index([name])
}
```
