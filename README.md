# Arno-prompts

Arno's Classical Prompts Collections to solve various kinds of problems.

## Prompt Template

- see -> `prompt.tpl.md`

## Learn to write prompts

* [Learning Prompt Wiki](https://learningprompt.wiki/)

## Prompt Patterns

Basic Patterns:

* zero-shot
* one / few-shot

### Reasoning

![](https://www.promptingguide.ai/_next/image?url=%2F_next%2Fstatic%2Fmedia%2FTOT.3b13bc5e.png&w=3840&q=75)

* chain of thought: **think step by step** offering reasoning process
* self-consistency
* [tree of thoughts](https://www.promptingguide.ai/techniques/tot)


```
Consider a multi-step reasoning problem like the following:

Question: If a store has 10 apples and 8 oranges, and it sells 6 apples and 4 oranges, how many fruits are left in the store?

Instead of directly answering the question, the chain-of-thought prompting would require the language model to produce a series of short sentences that mimic a human's reasoning process:

The store has 10 apples.
The store has 8 oranges.
6 apples are sold.
4 oranges are sold.
There are 10 - 6 = 4 apples left.
There are 8 - 4 = 4 oranges left.
The store now has 4 apples + 4 oranges = 8 fruits.

```

Using self-consistency, the language model generates multiple reasoning paths.

### RAG

This is a complex topic.

General-purpose language models can be fine-tuned to achieve several common tasks such as sentiment analysis and named entity recognition. These tasks generally don't require additional background knowledge.

For more complex and knowledge-intensive tasks, it's possible to build a language model-based system that accesses external knowledge sources to complete tasks. This enables more factual consistency, improves reliability of the generated responses, and helps to mitigate the problem of "hallucination".

### PAL

Use lang-chain or related tool / framework to systematically interact with the model.

### Tools & Functions

* Math
* Data
* Plot
* ...

## Prompt List

```
.
├── README.md
├── auto-gpt-prompt.summary.md
├── auto-gpt.prompt.raw.md
├── basic-prompt-framework.arno.md
├── chain-of-thought.arno.md
├── coze-bilibili-assistant
├── coze-official-prompt-tpl.md
├── crispe-prompt-framework.arno.md
├── dalle-3.md
├── information
│   ├── html-long-context-crawler.md
│   ├── html-long-context-table-generator.md
│   └── info-extractor.md
├── inversetment
│   ├── general-conception-explainer.md
│   ├── general-stock-investment-advisor.md
│   └── samples
│       └── stock-key-index-comparator.md
├── lifestyle
│   ├── cancer-dignostic-helper.md
│   └── nutrient-advisor.md
├── midjourney
│   ├── avatar
│   │   ├── manga.md
│   │   └── sa.md
│   ├── icon
│   │   └── ai-studio.md
│   ├── midj-generator.md
│   └── portrait
│       └── anime-teacher.md
├── patterns
│   ├── basic-prompt-framework.pattern.md
│   ├── crispe.prompt.framework.pattern.md
│   ├── promgramming.pattern.md
│   └── react.prompt.framework.pattern.md
├── programming
│   ├── biz
│   │   ├── crawler.md
│   │   └── front-end-code-assistant.md
│   ├── css
│   │   ├── css-advisor.md
│   │   └── css-assistant.md
│   ├── db
│   │   ├── instances
│   │   │   └── json2prisma.r1.md
│   │   ├── json2prisma.md
│   │   ├── prisma-assistant.md
│   │   ├── prisma-schema-designer.md
│   │   └── prisma-validator.md
│   ├── deisgn
│   │   └── plantuml.md
│   ├── fe
│   │   ├── antd
│   │   │   ├── antd-component-bot.md
│   │   │   ├── antd-component-converter.md
│   │   │   ├── antd-form-generator.md
│   │   │   ├── antd-general-component-writter.md
│   │   │   ├── antd-info-fields.md
│   │   │   ├── antd-pro-table-component.md
│   │   │   ├── antd-statistic-info-generator.md
│   │   │   ├── examples
│   │   │   └── prisma-schema2form.md
│   │   ├── editor
│   │   │   └── prose-mirror.dev.md
│   │   ├── fe-general-animation.md
│   │   ├── fe-general-assistant.md
│   │   ├── react
│   │   │   └── react-helper.md
│   │   └── vscode-assistant.md
│   ├── full-stack
│   │   ├── client-table-crud-operation.md
│   │   ├── curd-client-detail-page.md
│   │   ├── general-ui-component-generator.md
│   │   ├── nlp-2-prisma-schema.md
│   │   ├── primsa-schema-based-service-generator.md
│   │   ├── prisma-nextjs-api-route-resource-get-update.md
│   │   ├── prisma-schema-based-api-generator.md
│   │   ├── sever-page-generator.md
│   │   ├── ui-form-drawer-update-and-create.md
│   │   └── x-state
│   │       ├── x-state-helper.bot.md
│   │       └── x-state-vision.md
│   ├── json
│   │   └── json-operator.md
│   ├── nextjs
│   │   └── nextjs-assistant.md
│   ├── node.js
│   │   ├── code-enhancement.md
│   │   ├── general-function.writer.md
│   │   └── node-script.md
│   ├── qa
│   │   └── general.question.query.md
│   ├── server
│   │   └── nginx
│   │       ├── nginx-config.md
│   │       └── nginx-validator.md
│   ├── shell
│   │   ├── instances
│   │   │   └── shell-modify.md
│   │   ├── shell-assistant.md
│   │   └── shell-auto-corrention.md
│   ├── sql
│   │   ├── sql-assistant.md
│   │   ├── sql-generator.md
│   │   └── sql-prisma-based.md
│   ├── tests
│   │   ├── instances
│   │   │   └── unit-test.for-browser.context.md
│   │   ├── unit-test-enhance.nodejs.md
│   │   └── unit-test.nodejs.md
│   ├── typescript
│   │   ├── general-ts-assistant.md
│   │   ├── instances
│   │   │   └── date-calculator.md
│   │   ├── ts-converter.md
│   │   ├── ts-function-writter.md
│   │   ├── ts-re-uglify.md
│   │   └── ts-typing-assistant.md
│   ├── vite
│   │   └── vite-config-master.md
│   └── webpack
│       └── general-webpack-helper.md
├── prompt.tpl.md
├── prompts
│   ├── prompt-advisor.md
│   ├── prompt-optimizer.md
│   └── prompt-writter.md
├── re-act.prompt-framework.arno.md
├── stock-wiz.md
├── thinking
│   └── mental-model.md
├── web3
│   ├── ether-scan.md
│   └── ethernum.bot.md
└── writting
    ├── article-reviser.md
    └── article-translator-helper.md
```