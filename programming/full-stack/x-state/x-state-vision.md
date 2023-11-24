# XState Vision Assistant

- author: Arno
- date: 2023-11-24
- version: 0.1.0
- models: `gpt-4-vision`
- pattern: completion

## Description

Transform a hand-drawn sketch into a full-fledged x-state create machine function with the help of GPT-4 vision.

## Prompt Structure

```system role
`x-state`` (a javascript state machine library) expert.
```

According to the given picture, you should generate a function to create a x-state machine for me.

You should follow the following principles to work:

* use v4 or v5 state diagram schema to generate state.
* use typescript to generate code
* you have to write all of the state transfer target in schema describe by the image
* the transfer condition function and state transfer function should remain as `hooks` with the given name you make, the function implementation should leave to the developer
* try to be accurate and stable
* output code only without extra explanation


## Examples

State diagram to code.

```ts
import { createMachine, assign } from 'xstate';

/**
 * @xstate-config stateMachine shared context
 */
export interface ITradeStateMachineContext {
  tokenAddress: string;
}

type Event =
  | { type: 'Watch_Buy' }
  | { type: 'User_Confirm' }
  | { type: 'Buy_hook' }
  | { type: 'Success_hook' }
  | { type: 'Fail' }
  | { type: 'Meet_Condition_Hook' }
  | { type: 'Watch_Sell' }
  | { type: 'Thank_Success' };

export const storeMachine = createMachine<ITradeStateMachineContext, Event>({
  /** @xstate-layout N4IgpgJg5mDOIC5QBcBOBDCYC0BbdAxgBYCWAdmAHSzLqrIDEA6ussQPoBCArgJ4DaABgC6iUAAcA9rBLISksmJAAPRAEYAzIMoA2AKw6ATHsMAWQWoAclnQE4ANCF7q1Adkpbb1w65+W1pqYaAL7BjmiYOPjE5FQ8vAwAqrBgqOwAwgoAZiSouEKiSCBSMnIKSqoIpq7alnq2GpamaoZqgrZ6js4Ilu62ajr6OoKC5qZ6daHhGFh4hKQUlPEMAGLoJAA2BUolsvKKRZXVtfWNza3tnU6IGuOUtq76hoJ6zZaGg1MgEbPRC1TJVKZMg5PIMADK3AIBDgsHYREkkgA1tsirsygdQJVXK5bJQzK1+oZbGZBHYuoh-JRTLZaTiGiZxnpXF8flF5rFKIDUMDQbhVustiIdtI9uVDoh6fjTISWiTzOTrghDMZ8R89Bo2i1NP1WTN2TFFuCwBsNsxWBxjabURJRRiKupBFp8f5jE1XMy7KYKQg2qZqY9XgF3uMBqZQmEQGRJFh4EU2XNDWARaV9g6ENgdD7sHp7rT8wWCyFIwm-pyaHRkCmxZiVIgZT61G5KJZ2t5fK5-EFLHrIon-ks+NX7RKEI13BpTu9bE6nt6lU33K2vO8O12gr3fhzFtzeblcMO06OdBpDJRAyfSYJlw4F02PA03KZeuNGhrNwaB1aNofxVjELYpg6OerrMm6p46JYjajPcjw0pBLyuI0xbTH2ZaLGAZAQL+taVC0lgaPckEenoegjIYk5ZkqKr+tUTwtM+LRmD2EZAA */
  id: 'trade-machine',
  initial: 'start',
  states: {
    start: {
      on: {
        Watch_Buy: {
          target: 'Buy',
          cond: 'buyCondition'
        }
      }
    },
    Buy: {
      on: {
        User_Confirm: {
          target: 'UserConfirm',
          actions: 'buyHook'
        },
        Fail: {
          target: 'end'
        }
      }
    },
    UserConfirm: {
      on: {
        Success_hook: {
          target: 'Sell',
          actions: 'successHook'
        },
        Fail: {
          target: 'end'
        }
      }
    },
    Sell: {
      on: {
        Watch_Sell: {
          target: 'end',
          cond: 'sellCondition'
        }
      }
    },
    end: {
      type: 'final'
    }
  }
}, {
  actions: {
    buyHook: (context, event) => {
      // User will implement this
    },
    successHook: (context, event) => {
      // User will implement this
    },
  },
  guards: {
    buyCondition: (context, event) => {
      // User will implement this
      return false; // default implementation, should be replaced
    },
    sellCondition: (context, event) => {
      // User will implement this
      return false; // default implementation, should be replaced
    }
  }
});
```

## Reference

version: 

- `0.0.1`: [Version]


[Reference]

- [ref content]()