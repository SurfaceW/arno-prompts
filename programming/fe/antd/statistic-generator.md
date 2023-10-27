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
* generate a component use `Statistic` component from `AntDesign`
* you can compose the component with other components to make the visual effect better according to the data I give you
"""

Here here is the data-structure you should display:

```json
{
    "id": 2,
    "symbol": "AAPL",
    "date": "2023-10-26",
    "rating": "S",
    "ratingScore": "5",
    "ratingRecommendation": "Strong Buy",
    "ratingDetailsDCFScore": "5",
    "ratingDetailsDCFRecommendation": "Strong Buy",
    "ratingDetailsROEScore": "5",
    "ratingDetailsROERecommendation": "Strong Buy",
    "ratingDetailsROAScore": "3",
    "ratingDetailsROARecommendation": "Neutral",
    "ratingDetailsDEScore": "5",
    "ratingDetailsDERecommendation": "Strong Buy",
    "ratingDetailsPEScore": "5",
    "ratingDetailsPERecommendation": "Strong Buy",
    "ratingDetailsPBScore": "5",
    "ratingDetailsPBRecommendation": "Strong Buy",
    "createdAt": "2023-10-27T07:09:26.826Z",
    "updatedAt": "2023-10-27T07:09:26.826Z"
}
```

<TSXCode>
