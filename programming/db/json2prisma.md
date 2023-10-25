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
  "symbol": "AAPL",
  "date": "2023-07-01",
  "calendarYear": "2023",
  "period": "Q3",
  "currentRatio": 0.9815625425125837,
  "quickRatio": 0.8135848211070477,
  "cashRatio": 0.22733129006185832,
  "daysOfSalesOutstanding": 174.85836888883458,
  "daysOfInventoryOutstanding": 14.57760444209413,
  "operatingCycle": 57.69336663386155,
  "daysOfPayablesOutstanding": 92.60774722369116,
  "cashConversionCycle": -34.91438058982961,
  "grossProfitMargin": 0.44516302553883397,
  "operatingProfitMargin": 0.2811594557257601,
  "pretaxProfitMargin": 0.2779197281073878,
  "netProfitMargin": 0.24305292370135825,
  "effectiveTaxRate": 0.12545638499098227,
  "returnOnAssets": 0.059339537604689616,
  "returnOnEquity": 0.32984371370740284,
  "returnOnCapitalEmployed": 0.10947518743305962,
  "netIncomePerEBT": 0.8745436150090178,
  "ebtPerEbit": 0.9884772588920776,
  "ebitPerRevenue": 0.2811594557257601,
  "debtRatio": 0.32617195661387666,
  "debtEquityRatio": 1.8130537213392175,
  "longTermDebtToCapitalization": 0.6193501531466102,
  "totalDebtToCapitalization": 0.6445144319803721,
  "interestCoverage": 23.044088176352705,
  "cashFlowToDebtRatio": 0.24139824304538798,
  "companyEquityMultiplier": 5.558582473371603,
  "receivablesTurnover": 2.0874036645740826,
  "payablesTurnover": 0.9718409387781323,
  "inventoryTurnover": 6.173853897428922,
  "fixedAssetTurnover": 1.878231917336395,
  "assetTurnover": 0.24414245548266167,
  "operatingCashFlowPerShare": 1.6805101718006317,
  "freeCashFlowPerShare": 1.5471778067673214,
  "cashPerShare": 3.980350134740222,
  "payoutRatio": 0.19360193149237967,
  "operatingCashFlowSalesRatio": 0.3225057153685343,
  "freeCashFlowOperatingCashFlowRatio": 0.9206595905989385,
  "cashFlowCoverageRatios": 0.24139824304538798,
  "shortTermCoverageRatios": 2.3534659648496743,
  "capitalExpenditureCoverageRatio": -12.603917821309127,
  "dividendPaidAndCapexCoverageRatio": 15.022779043280183,
  "dividendPayoutRatio": 0.19360193149237964,
  "priceBookValueRatio": 50.517075149815845,
  "priceToBookRatio": 50.517075149815845,
  "priceToSalesRatio": 37.224668234531826,
  "priceEarningsRatio": 38.28864478119813,
  "priceToFreeCashFlowsRatio": 125.37020577181208,
  "priceToOperatingCashFlowsRatio": 115.4232823191812,
  "priceCashFlowRatio": 115.4232823191812,
  "priceEarningsToGrowthRatio": -2.2531394813551207,
  "priceSalesRatio": 37.224668234531826,
  "dividendYield": 0.0012640949594764,
  "enterpriseValueMultiple": 135.9134788929472,
  "priceFairValue": 50.517075149815845
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
