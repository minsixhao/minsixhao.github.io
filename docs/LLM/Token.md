---
layout: default
title: Token
nav_order: 2
has_children: true
parent: 大模型相关
permalink: /docs/LLM/Token
---

## 什么是 Token
token 是自然语言处理的最细粒度，简单点说就是，GPT 的输入是一个个的 token，输出也是一个个的 token。当我们调用 OpenAI API 时，通过提示传递的输入都将被拆分为 token 来表示，这种把一句话提示的输入拆分的过程又叫分词(tokenization)。

分词的过程涉及将纯文本（如短语、句子、段落或整个文档）划分为更小的数据块，以便于分析。 这些更小的数据块被称为tokens，可能包括单词、短语或句子，且这些token不是完全按照单词的起始或结束位置拆分的，还可能包含尾随后的空格。

GPT 不是适用于某一门语言的大型语言模型，它适用于几乎所有流行的自然语言。所以 GPT 的 token 需要 兼容 几乎人类的所有自然语言，那意味着 GPT 有一个非常全的 token 词汇表，它能表达出所有人类的自然语言。如何实现这个目的呢？

> Byte Pair Encoding(BPE) 编码，他在 GPT-2 中被使用，BPE每一步都将最常见的一对相邻数据单位替换为该数据中没有出现过的一个新单位，反复迭代直到满足停止条件。

> - 详细请参考：https://zhuanlan.zhihu.com/p/424631681
> - Token在线编码转换：https://platform.openai.com/tokenizer

GPT 实际是将我们输入的文字转换成 token，然后通过 GPT 模型预测 token，再将 token 转换成文字，最后再输出给我们。

