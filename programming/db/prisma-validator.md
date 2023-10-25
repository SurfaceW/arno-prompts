check the following **prisma schema** is valid or not for database as **PostgreSQL**.

```prisma
generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider  = "postgresql"
  url       = env("POSTGRES_PRISMA_URL") // uses connection pooling
  directUrl = env("POSTGRES_URL_NON_POOLING") // uses a direct connection
}

model User {
  id        Int      @id @default(autoincrement())
  name      String
  email     String   @unique
  image     String
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
}

model Company {
  id                Int       @id @default(autoincrement())
  symbol            String    @unique
  price             Float?
  beta              Float?
  volAvg            Int?
  mktCap            BigInt?
  lastDiv           Float?
  range             String?
  changes           Float?
  companyName       String?
  currency          String?
  cik               String?
  isin              String?
  cusip             String?
  exchange          String?
  exchangeShortName String?
  industry          String?
  website           String?
  description       String?
  ceo               String?
  sector            String?
  country           String?
  fullTimeEmployees String?
  phone             String?
  address           String?
  city              String?
  state             String?
  zip               String?
  dcfDiff           Float?
  dcf               Float?
  image             String?
  ipoDate           DateTime?
  defaultImage      Boolean?
  isEtf             Boolean?
  isActivelyTrading Boolean?
  isAdr             Boolean?
  isFund            Boolean?
  createdAt         DateTime  @default(now())
  updatedAt         DateTime  @updatedAt

  rating CompanyRating?

  finCashFlowStatement     FinancialCashFlowStatement[]
  finIncomeStatement       FinancialIncomeStatement[]
  finBalanceSheetStatement FinancialBalanceSheetStatement[]
  finKeyMetricsStatement   FinancialKeyMetricsStatement[]
  finRatioStatement        FinancialRatioStatement[]
}

model CompanyRating {
  id                             Int     @id @default(autoincrement())
  symbol                         String
  date                           String
  rating                         String?
  ratingScore                    Int?
  ratingRecommendation           String?
  ratingDetailsDCFScore          Int?
  ratingDetailsDCFRecommendation String?
  ratingDetailsROEScore          Int?
  ratingDetailsROERecommendation String?
  ratingDetailsROAScore          Int?
  ratingDetailsROARecommendation String?
  ratingDetailsDEScore           Int?
  ratingDetailsDERecommendation  String?
  ratingDetailsPEScore           Int?
  ratingDetailsPERecommendation  String?
  ratingDetailsPBScore           Int?
  ratingDetailsPBRecommendation  String?

  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt

  company Company @relation(fields: [id], references: [id])
}

model FinancialCashFlowStatement {
  id                                       Int     @id @default(autoincrement())
  symbol                                   String
  date                                     String?
  reportedCurrency                         String?
  cik                                      String?
  fillingDate                              String?
  acceptedDate                             String?
  calendarYear                             String?
  period                                   String?
  netIncome                                Int?
  depreciationAndAmortization              Int?
  deferredIncomeTax                        Int?
  stockBasedCompensation                   Int?
  changeInWorkingCapital                   Int?
  accountsReceivables                      Int?
  inventory                                Int?
  accountsPayables                         Int?
  otherWorkingCapital                      Int?
  otherNonCashItems                        Int?
  netCashProvidedByOperatingActivities     Int?
  investmentsInPropertyPlantAndEquipment   Int?
  acquisitionsNet                          Int?
  purchasesOfInvestments                   Int?
  salesMaturitiesOfInvestments             Int?
  otherInvestingActivites                  Int?
  netCashUsedForInvestingActivites         Int?
  debtRepayment                            Int?
  commonStockIssued                        Int?
  commonStockRepurchased                   Int?
  dividendsPaid                            Int?
  otherFinancingActivites                  Int?
  netCashUsedProvidedByFinancingActivities Int?
  effectOfForexChangesOnCash               Int?
  netChangeInCash                          Int?
  cashAtEndOfPeriod                        Int?
  cashAtBeginningOfPeriod                  Int?
  operatingCashFlow                        Int?
  capitalExpenditure                       Int?
  freeCashFlow                             Int?
  link                                     String?
  finalLink                                String?

  company Company @relation(fields: [symbol], references: [symbol])
}

model FinancialIncomeStatement {
  id                                      Int     @id @default(autoincrement())
  symbol                                  String
  date                                    String?
  reportedCurrency                        String?
  cik                                     String?
  fillingDate                             String?
  acceptedDate                            String?
  calendarYear                            String?
  period                                  String?
  revenue                                 Int?
  costOfRevenue                           Int?
  grossProfit                             Int?
  grossProfitRatio                        Float?
  researchAndDevelopmentExpenses          Int?
  generalAndAdministrativeExpenses        Int?
  sellingAndMarketingExpenses             Int?
  sellingGeneralAndAdministrativeExpenses Int?
  otherExpenses                           Int?
  operatingExpenses                       Int?
  costAndExpenses                         Int?
  interestIncome                          Int?
  interestExpense                         Int?
  depreciationAndAmortization             Int?
  ebitda                                  Int?
  ebitdaratio                             Float?
  operatingIncome                         Int?
  operatingIncomeRatio                    Float?
  totalOtherIncomeExpensesNet             Int?
  incomeBeforeTax                         Int?
  incomeBeforeTaxRatio                    Float?
  incomeTaxExpense                        Int?
  netIncome                               Int?
  netIncomeRatio                          Float?
  eps                                     Float?
  epsdiluted                              Float?
  weightedAverageShsOut                   Int?
  weightedAverageShsOutDil                Int?
  link                                    String?
  finalLink                               String?

  company Company @relation(fields: [symbol], references: [symbol])
}

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
  cashAndCashEquivalents                  Int?
  shortTermInvestments                    Int?
  cashAndShortTermInvestments             Int?
  netReceivables                          Int?
  inventory                               Int?
  otherCurrentAssets                      Int?
  totalCurrentAssets                      Int?
  propertyPlantEquipmentNet               Int?
  goodwill                                Int?
  intangibleAssets                        Int?
  goodwillAndIntangibleAssets             Int?
  longTermInvestments                     Int?
  taxAssets                               Int?
  otherNonCurrentAssets                   Int?
  totalNonCurrentAssets                   Int?
  otherAssets                             Int?
  totalAssets                             Int?
  accountPayables                         Int?
  shortTermDebt                           Int?
  taxPayables                             Int?
  deferredRevenue                         Int?
  otherCurrentLiabilities                 Int?
  totalCurrentLiabilities                 Int?
  longTermDebt                            Int?
  deferredRevenueNonCurrent               Int?
  deferredTaxLiabilitiesNonCurrent        Int?
  otherNonCurrentLiabilities              Int?
  totalNonCurrentLiabilities              Int?
  otherLiabilities                        Int?
  capitalLeaseObligations                 Int?
  totalLiabilities                        Int?
  preferredStock                          Int?
  commonStock                             Int?
  retainedEarnings                        Int?
  accumulatedOtherComprehensiveIncomeLoss Int?
  othertotalStockholdersEquity            Int?
  totalStockholdersEquity                 Int?
  totalEquity                             Int?
  totalLiabilitiesAndStockholdersEquity   Int?
  minorityInterest                        Int?
  totalLiabilitiesAndTotalEquity          Int?
  totalInvestments                        Int?
  totalDebt                               Int?
  netDebt                                 Int?
  link                                    String?
  finalLink                               String?

  company Company @relation(fields: [symbol], references: [symbol])
}

model FinancialKeyMetricsStatement {
  id                                     Int     @id @default(autoincrement())
  symbol                                 String
  date                                   String?
  calendarYear                           String?
  period                                 String?
  revenuePerShare                        Float?
  netIncomePerShare                      Float?
  operatingCashFlowPerShare              Float?
  freeCashFlowPerShare                   Float?
  cashPerShare                           Float?
  bookValuePerShare                      Float?
  tangibleBookValuePerShare              Float?
  shareholdersEquityPerShare             Float?
  interestDebtPerShare                   Float?
  marketCap                              Int?
  enterpriseValue                        Int?
  peRatio                                Float?
  priceToSalesRatio                      Float?
  pocfratio                              Float?
  pfcfRatio                              Float?
  pbRatio                                Float?
  ptbRatio                               Float?
  evToSales                              Float?
  enterpriseValueOverEBITDA              Float?
  evToOperatingCashFlow                  Float?
  evToFreeCashFlow                       Float?
  earningsYield                          Float?
  freeCashFlowYield                      Float?
  debtToEquity                           Float?
  debtToAssets                           Float?
  netDebtToEBITDA                        Float?
  currentRatio                           Float?
  interestCoverage                       Float?
  incomeQuality                          Float?
  dividendYield                          Float?
  payoutRatio                            Float?
  salesGeneralAndAdministrativeToRevenue Float?
  researchAndDdevelopementToRevenue      Float?
  intangiblesToTotalAssets               Float?
  capexToOperatingCashFlow               Float?
  capexToRevenue                         Float?
  capexToDepreciation                    Float?
  stockBasedCompensationToRevenue        Float?
  grahamNumber                           Float?
  roic                                   Float?
  returnOnTangibleAssets                 Float?
  grahamNetNet                           Float?
  workingCapital                         Int?
  tangibleAssetValue                     Int?
  netCurrentAssetValue                   Int?
  investedCapital                        Float?
  averageReceivables                     Int?
  averagePayables                        Int?
  averageInventory                       Int?
  daysSalesOutstanding                   Float?
  daysPayablesOutstanding                Float?
  daysOfInventoryOnHand                  Float?
  receivablesTurnover                    Float?
  payablesTurnover                       Float?
  inventoryTurnover                      Float?
  roe                                    Float?
  capexPerShare                          Float?

  // keep relationship with company
  company Company @relation(fields: [symbol], references: [symbol])
}

model FinancialRatioStatement {
  id                                 Int     @id @default(autoincrement())
  symbol                             String
  date                               String?
  calendarYear                       String?
  period                             String?
  currentRatio                       Float?
  quickRatio                         Float?
  cashRatio                          Float?
  daysOfSalesOutstanding             Float?
  daysOfInventoryOutstanding         Float?
  operatingCycle                     Float?
  daysOfPayablesOutstanding          Float?
  cashConversionCycle                Float?
  grossProfitMargin                  Float?
  operatingProfitMargin              Float?
  pretaxProfitMargin                 Float?
  netProfitMargin                    Float?
  effectiveTaxRate                   Float?
  returnOnAssets                     Float?
  returnOnEquity                     Float?
  returnOnCapitalEmployed            Float?
  netIncomePerEBT                    Float?
  ebtPerEbit                         Float?
  ebitPerRevenue                     Float?
  debtRatio                          Float?
  debtEquityRatio                    Float?
  longTermDebtToCapitalization       Float?
  totalDebtToCapitalization          Float?
  interestCoverage                   Float?
  cashFlowToDebtRatio                Float?
  companyEquityMultiplier            Float?
  receivablesTurnover                Float?
  payablesTurnover                   Float?
  inventoryTurnover                  Float?
  fixedAssetTurnover                 Float?
  assetTurnover                      Float?
  operatingCashFlowPerShare          Float?
  freeCashFlowPerShare               Float?
  cashPerShare                       Float?
  payoutRatio                        Float?
  operatingCashFlowSalesRatio        Float?
  freeCashFlowOperatingCashFlowRatio Float?
  cashFlowCoverageRatios             Float?
  shortTermCoverageRatios            Float?
  capitalExpenditureCoverageRatio    Float?
  dividendPaidAndCapexCoverageRatio  Float?
  dividendPayoutRatio                Float?
  priceBookValueRatio                Float?
  priceToBookRatio                   Float?
  priceToSalesRatio                  Float?
  priceEarningsRatio                 Float?
  priceToFreeCashFlowsRatio          Float?
  priceToOperatingCashFlowsRatio     Float?
  priceCashFlowRatio                 Float?
  priceEarningsToGrowthRatio         Float?
  priceSalesRatio                    Float?
  dividendYield                      Float?
  enterpriseValueMultiple            Float?
  priceFairValue                     Float?

  // keep relationship with company
  company Company @relation(fields: [symbol], references: [symbol])
}

```

* if it is valid, return `true`.
* if it is not, give the suggestion to fix it with list.
