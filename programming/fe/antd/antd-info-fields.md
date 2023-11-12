# AntD Component Generator BOT version

- author: Arno
- date: 2023-11-12
- version: 0.0.1
- models: gpt-4-32k(better)
- pattern: chatbot

## Prompt Structure

Here are your instructions:

1. use AntDesign Component Library to create a component
2. use TypeScript version 5
3. use React Hook Style
4. generate the text start with ```tsx and end with ```
5. export defined interface
6. do not use `export default` syntax, use `export const` instead
7. add `'use client';` on the first line of the code
8. use the standard `useSwr` or `useMutation` from swr lib API if you need
9. **do output code without explanation**

Based on the context given below work:

```
{
  "statusCode": 200,
  "data": {
    "audit": {
      "is_contract_renounced": true,
      "codeVerified": true,
      "date": "2023-11-12T14:40:17.454Z",
      "lockTransactions": false,
      "mint": false,
      "provider": "GoPlus",
      "proxy": false,
      "status": "OK",
      "unlimitedFees": false,
      "version": 1
    },
    "decimals": 18,
    "info": {
      "description": "",
      "email": "",
      "extraInfo": "",
      "nftCollection": "",
      "ventures": false
    },
    "links": {
      "bitbucket": "",
      "discord": "",
      "facebook": "",
      "github": "",
      "instagram": "",
      "linkedin": "",
      "medium": "",
      "reddit": "",
      "telegram": "",
      "tiktok": "",
      "twitter": "",
      "website": "",
      "youtube": ""
    },
    "logo": "",
    "metrics": {
      "maxSupply": 6900000000,
      "totalSupply": 6900000000,
      "holders": 23,
      "txCount": 34
    },
    "name": "Bibo",
    "symbol": "BIBO",
    "totalSupply": "6900000000000000000000000000",
    "creationBlock": 18556366,
    "reprPair": {
      "id": {
        "chain": "ether",
        "exchange": "univ2",
        "pair": "0x4c480c0bad0ceb725542366c64da065b1df2ce56",
        "token": "0xbe7a294e1e4579c755aea702b298a2b0fda92303",
        "tokenRef": "0xc02aaa39b223fe8d0a0e5c4f27ead9083c756cc2"
      },
      "price": 0.0000015501969347754543
    },
    "pairs": [
      {
        "address": "0x4c480c0bad0ceb725542366c64da065b1df2ce56",
        "exchange": "univ2",
        "dextScore": 0,
        "price": 0.0000015501969347754543,
        "tokenRef": {
          "address": "0xc02aaa39b223fe8d0a0e5c4f27ead9083c756cc2",
          "name": "Wrapped Ether",
          "symbol": "WETH"
        }
      }
    ],
    "chain": "ether",
    "address": "0xbe7a294e1e4579c755aea702b298a2b0fda92303"
  }
}
```
You have to display these data in a page component, try to be a good designer to accurately display the data in the page.
You can use `Description` component and `List` component or `Table` component in `antd` to display the data.
You should use multiple section to separate the data, and use `Divider` component to separate the sections.
You do not need to fetch remote data, use data from its props.
This information is about block-chain token security, related data, some field may be hard to understand, you can add tip on it with some description if you know.

<TSXCode>

## Examples and Instances

WIP

## Reference