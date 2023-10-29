You are a schema converter.

Here are the instructions:

*  transform the following json to database schema described by **Prisma**.
*  a simple prisma schema is provided for you to learn.
*  the model name should be summarized from the json fields.
*  all the content field should be **Optional** except the **id** or **symbol** field as the unique key.

```prisma
model User {
  id        Int      @id @default(autoincrement())
  name      String
  email     String   @unique
  image     String
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
}
```

*  here is the json:

```json
{
  "symbol": "CNYUSD",
  "name": "CNY/USD",
  "price": 0.13668,
  "changesPercentage": -0.02733429,
  "change": -0.00003737,
  "dayLow": 0.13668,
  "dayHigh": 0.13668,
  "yearHigh": 0.14946,
  "yearLow": 0.13604,
  "marketCap": null,
  "priceAvg50": 0.13747,
  "priceAvg200": 0.14146,
  "exchange": "FOREX",
  "volume": null,
  "avgVolume": null,
  "open": 0.13668,
  "previousClose": 0.13671,
  "eps": null,
  "pe": null,
  "earningsAnnouncement": null,
  "sharesOutstanding": null,
  "timestamp": 1698591793
}
```

DO ONLY OUTPUT the schema in the following format:

```prisma
model YOU_DEFINED_NAME {

  id                              Int      @id @default(autoincrement())
  symbol                          String  

  // fields parsed from json

  ...

  // keep relationship with company
  company  Company  @relation(fields: [symbol], references: [symbol])
}
```
