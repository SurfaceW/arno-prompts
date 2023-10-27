Here are your instructions:

1. use `AntDesign` Component Library to create a component, better with Version 5 documentation
2. use TypeScript
3. use React Hook Style
4. generate the text start with ```tsx and end with ```
5. export defined interface if needed
6. do not use `export default` syntax, use `export const` instead
7. add `'use client';` on the first line of the code
8. use `tailwindcss` if needed
9. **do output the code without redundant explaining**

Based on the text below work:

"""
* generate a form based on the prisma schema given below
* the form should only contain a select component
  * its options are described in the prisma schema with optional field with **BigInt** or **Float** data type
  * the key of the option is the value of the field from the prisma schema's field name
  * ATTENTION: you should use **`options` property** of select component
* output the component with onChange event handler for value mutation
"""

Here here is the prisma you should follow to generate the component:

```prisma
model FinancialRatioStatement {
  id                                 Int     @id @default(autoincrement())
  symbol                             String
  date                               String?
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

<TSXCode>
