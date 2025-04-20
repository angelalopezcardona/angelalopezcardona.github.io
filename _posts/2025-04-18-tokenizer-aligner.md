---
layout: post
title: Aligning Tokens between different LLM Tokenizers
date: 2025-04-18 14:37:00-0400
description: tokenizer aligner python library explained
tags: llms
categories: llm
giscus_comments: true
related_posts: true
pretty_table: true
---



Language models like GPT, Gemini, or Claude have emerged as powerful tools for processing textual representations and modeling complex relationships between input features and output labels. Today, their use has become widespread, with millions of users relying on them in their daily lives.

While the internal workings and architectures of these models are beyond the scope of this blog, it's important to understand one key concept: the first step in most language models involves converting input text into tokens. These tokens are then mapped to learned vector representations called embeddings. Each model uses its own tokenization algorithm and, during training, learns the embeddings for each token in its vocabulary.

Since each model uses a different tokenizer, the same input text can be split into slightly different token sets across models. So what happens when you want to share token-level or embedding-level information between different models? In our experience, you've got a problem—because there's no straightforward way to align them 😊.

After searching for existing solutions, we found that none were available—not even in the amazing [*transformers*](https://huggingface.co/docs/transformers/en/index) library. So, we built our [own solution](https://github.com/angelalopezcardona/tokenizer_aligner) and released it on GitHub!


# tokenizer_aligner
Python library for mapping tokens between different tokenizers

## Modules

To map between tokenisers, first, we perform an initial mapping of tokens to the words they belong to in each tokenizer with some properties of [*FastTokenizers*](https://huggingface.co/docs/tokenizers/index) from the [*Transformers*](https://huggingface.co/docs/transformers/en/index) library. Then, we map words from one tokenizer to the words in the other and finally, we assume that the combination of the tokens that are mapped to a word in one tokenizer correspond to the tokens that are mapped to the word that is mapped to the initial word in the other tokenizer. 

**Token Mapping Example**

|  | Tokenizer 1 | Tokenizer 2 |
|-------|-------------|-------------|
| Words | astrophotography | astrophotography |
| Tokens | ['_Astro', 'photo', 'graphy'] | ['Ċ', 'Ast', 'roph', 'ot', 'ography'] |
| Token indices | [22, 23, 24] | [23, 24, 25, 26, 271] |
| Token IDs | [15001, 17720, 16369] | [198, 62152, 22761, 354, 5814] |


## Installation

```sh
pip install git+https://github.com/angelalopezcardona/tokenizer_aligner.git
```