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
  "code": 1,
  "message": "OK",
  "result": {
    "0xdd466e9c060694aff06abdde57615282d7239562": {
      "anti_whale_modifiable": "0",
      "buy_tax": "0",
      "can_take_back_ownership": "0",
      "cannot_buy": "0",
      "cannot_sell_all": "0",
      "creator_address": "0x15ea7c32526076ccc06a62383e6e654c11147cc5",
      "creator_balance": "0",
      "creator_percent": "0.000000",
      "dex": [
        {
          "name": "UniswapV2",
          "liquidity": "2762.38854260",
          "pair": "0x807df86b367411fb31ba88c97d2baa2f9575f52e"
        }
      ],
      "external_call": "0",
      "hidden_owner": "0",
      "holder_count": "158",
      "holders": [
        {
          "address": "0x807df86b367411fb31ba88c97d2baa2f9575f52e",
          "tag": "UniswapV2",
          "is_contract": 1,
          "balance": "704706375.811057689",
          "percent": "0.704706375811057689",
          "is_locked": 0
        },
      ],
      "honeypot_with_same_creator": "0",
      "is_anti_whale": "1",
      "is_blacklisted": "0",
      "is_honeypot": "0",
      "is_in_dex": "1",
      "is_mintable": "0",
      "is_open_source": "1",
      "is_proxy": "0",
      "is_whitelisted": "1",
      "lp_holder_count": "2",
      "lp_holders": [
        {
          "address": "0xe2fe530c047f2d85298b07d9333c05737f1435fb",
          "tag": "TeamFinance",
          "is_contract": 1,
          "balance": "0.948683298050512799",
          "percent": "0.999999999999998945",
          "is_locked": 1
        },
      ],
      "lp_total_supply": "0.948683298050513799",
      "owner_address": "0x0000000000000000000000000000000000000000",
      "owner_balance": "0",
      "owner_change_balance": "0",
      "owner_percent": "0.000000",
      "personal_slippage_modifiable": "0",
      "selfdestruct": "0",
      "sell_tax": "0",
      "slippage_modifiable": "1",
      "token_name": "Jujutsu Kaisen",
      "token_symbol": "JUJU",
      "total_supply": "1000000000",
      "trading_cooldown": "0",
      "transfer_pausable": "0"
    }
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