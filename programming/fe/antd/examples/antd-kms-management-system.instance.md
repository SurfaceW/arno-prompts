* bot: https://poe.com/antdplusnextjs
* template ref -> `antd-component-bot.md`

# Knowledge Set with Entity CRUD

## Knowledge Set Detail Page

ai: https://poe.com/chat/2spbszu0rgmqrupc3qt

* generate knowledge set detail section with header of set name
* the set data structure is described by the prisma below:

```prisma
model KnowledgeSet {
  id        BigInt   @id @default(autoincrement())
  cuid      String   @unique @default(cuid())
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
  desc      String?  @db.MediumText
  name      String   @db.VarChar(100)
  status    Int      @default(0)
  createdBy String?  @db.VarChar(120)
  updatedBy String?  @db.VarChar(120)
  type      Int      @default(0)

  content Json? @db.Json

  app   AIApp  @relation(fields: [appId], references: [id])
  appId BigInt

  entities KnowledgeEntity[]

  @@index([appId])
}
```

* try to display the basic knowledge set information with `Descriptions` component with fields
  * content of the JSON can be ignore and implement it later with some other independent component
* generate a default demo data for default value when props of knowledge entity data is not provided
* for entities we don't need to display, and implement it later with some other independent component

# Knowledge Entity Workflow

## Workflow Steps

ai: https://poe.com/chat/2spa7id3oqklluzn4ck

* generate a step component with the following steps
  * step 1: Source file management
    * description: manage the knowledge entity source file
  * step 2: Source file parsing
    * description: parse the source file including its image and video ...
  * step 3: Source file content annotation
    * description: annotate the source file content with AI
  * step 4: Knowledge entity indexing
    * description: index the knowledge entity with embedding and indexing engine
* each step is clickable when user click the step, the step will be highlighted and trigger the callback function
* each step should have a unique icon according to its description and pick from @ant-design/icons to render the icon properly
* step item should not have ordered number, and can be switched to any other step


## Knowledge Entity Table

ai: 

Generate a knowledge entity table below to a knowledge set described by the prisma schema below:

```prisma
model KnowledgeEntity {
  id        BigInt   @id @default(autoincrement())
  cuid      String   @unique @default(cuid())
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
  name      String   @db.VarChar(100)
  status    Int      @default(0)
  createdBy String?  @db.VarChar(120)
  updatedBy String?  @db.VarChar(120)
  type      Int      @default(0)

  content Json? @db.Json

  knowledgeSet   KnowledgeSet @relation(fields: [knowledgeSetId], references: [id])
  knowledgeSetId BigInt

  @@index([knowledgeSetId])
}
```

and the Content with more information is described by the typescript interface below:

```ts
interface IKnowledgeEntityContent {
  file: {
    name: string;
    url: string;
    size: number; 
    type: string; // pdf, ppt, doc, docx, xls, xlsx, jpg, png, mp4, avi, mov, wav, mp3, ...
  };
  type: string;
}
```

Try to render file icon with different icon. with csv, xls, pdf, markdown, text and ppt and others.

The table should show all those fields above in the table columns.

The status should use `Status` component to render the status with different color infer with below.

* 0 `SourceUpdated`
* 1 `Parsed`
* 2 `Annotated`
* 3 `Indexed`
* 4 `Failed`
* 5 `Deleted`

And the type is described by the enum below:

```ts
enum KnowledgeEntityType {
  Text = 0,
  Image = 1,
  Video = 2,
  Audio = 3,
}
```


