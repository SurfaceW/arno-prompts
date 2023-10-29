I have prisma schema here: 

```prisma
model FinancialBalanceSheetStatement {
  id     Int    @id @default(autoincrement())
  symbol String

  date                                    String?
  reportedCurrency                        String?
  cik                                     String?
  fillingDate                             String?
  acceptedDate                            String?
  calendarYear                            String?
  period                                  String?
  cashAndCashEquivalents                  BigInt?
  shortTermInvestments                    BigInt?
  cashAndShortTermInvestments             BigInt?
  netReceivables                          BigInt?
  inventory                               BigInt?
  otherCurrentAssets                      BigInt?
  totalCurrentAssets                      BigInt?
  propertyPlantEquipmentNet               BigInt?
  goodwill                                BigInt?
  intangibleAssets                        BigInt?
  goodwillAndIntangibleAssets             BigInt?
  longTermInvestments                     BigInt?
  taxAssets                               BigInt?
  otherNonCurrentAssets                   BigInt?
  totalNonCurrentAssets                   BigInt?
  otherAssets                             BigInt?
  totalAssets                             BigInt?
  accountPayables                         BigInt?
  shortTermDebt                           BigInt?
  taxPayables                             BigInt?
  deferredRevenue                         BigInt?
  otherCurrentLiabilities                 BigInt?
  totalCurrentLiabilities                 BigInt?
  longTermDebt                            BigInt?
  deferredRevenueNonCurrent               BigInt?
  deferredTaxLiabilitiesNonCurrent        BigInt?
  otherNonCurrentLiabilities              BigInt?
  totalNonCurrentLiabilities              BigInt?
  otherLiabilities                        BigInt?
  capitalLeaseObligations                 BigInt?
  totalLiabilities                        BigInt?
  preferredStock                          BigInt?
  commonStock                             BigInt?
  retainedEarnings                        BigInt?
  accumulatedOtherComprehensiveIncomeLoss BigInt?
  othertotalStockholdersEquity            BigInt?
  totalStockholdersEquity                 BigInt?
  totalEquity                             BigInt?
  totalLiabilitiesAndStockholdersEquity   BigInt?
  minorityInterest                        BigInt?
  totalLiabilitiesAndTotalEquity          BigInt?
  totalInvestments                        BigInt?
  totalDebt                               BigInt?
  netDebt                                 BigInt?
  link                                    String?
  finalLink                               String?

  company Company @relation(fields: [symbol], references: [symbol])
}

```

This is the code I want you to optimize:

```typescript
  async getCompanyBalanceSheetStatement(symbol: string, period?: 'quarter' | 'annual') {
    return this._queryCompanyRelatedStatementByPeriod(
      symbol,
      period || 'annual',
      'balance-sheet-statement',
      this._prismaClient.financialBalanceSheetStatement,
      {
        limit: 100,
        orderBy: {
          date: 'asc',
        },
        apiDataProcess: (data) => ({
          ...data,
          depreciationAndAmortization:
            BigInt(parseInt(data.depreciationAndAmortization || 0, 10)) || BigInt(0),
        }),
      }
    );
```

Now I want you to edit `apiDataProcess` to handle some BigInt type receive Float data-type cause prisma insert data error.
You have to write some adapter like `depreciationAndAmortization` field did to ensure the durability.

Just output typescript code:

<Typescript>