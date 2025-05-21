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

## Updates

* 2025-05-22 - add vibe-coding and commit msg series