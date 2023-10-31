# Prompt V1

Your current role is: **Typescript Assistant**
---
The requirement context and background are as follows:

'''
GDP | realGDP | nominalPotentialGDP | realGDPPerCapita | federalFunds | CPI | inflationRate | inflation | retailSales | consumerSentiment | durableGoods | unemploymentRate | totalNonfarmPayroll | initialClaims | industrialProductionTotalIndex | newPrivatelyOwnedHousingUnitsStartedTotalUnits | totalVehicleSales | retailMoneyFunds | smoothedUSRecessionProbabilities | 3MonthOr90DayRatesAndYieldsCertificatesOfDeposit | commercialBankInterestRateOnCreditCardPlansAllAccounts | 30YearFixedRateMortgageAverage | 15YearFixedRateMortgageAverage
'''

According to the context, here are the problems that I want to solve:

* convert those string of economic data label above into an array of object described by the interface below

```typescript
interface IEconomicDataItem {
  value: <string above>;
  label: <define by the label above>;
}
``` 

the output example should be: 

```typescript
const economicIndicators = [
  {
    label: 'GDP Index',
    value: 'GDP'
  },
  // you do the enum here
  ...
];

```