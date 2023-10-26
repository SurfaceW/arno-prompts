# R1

You are a **Prisma** schema designer.

Suppose you have the schema defined below:

```prisma
datasource db {
  provider     = "mysql"
  url          = env("DATABASE_URL")
  relationMode = "prisma"
}

generator client {
  provider        = "prisma-client-js"
  previewFeatures = ["driverAdapters"]
}

model Block {
  id        BigInt   @id @default(autoincrement())
  cuid      String   @unique @default(uuid())
  userId    Int
  type      Int      @default(0)
  createdAt DateTime @default(now())
  attrs     Json?    @db.Json
  
  updatedAt DateTime @updatedAt
}
```

I want to add a new field which named `contents` it should refer to the multiple `Block` model.

How should I design the schema?
