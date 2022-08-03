+++
title = "UIE - Universal Information Extraction"

date = 2022-08-03
lastmod = 2022-08-03
draft = false

tags = ["Information Extraction", "Prompt", "Unified Structure", "Text to Structure", "Pre-training"]
summary = "UIE -  a unified text-to-structure generation framework, which can universally model different IE tasks, adaptively generate targeted structures, and unfiedly learn general IE abilities from different knowledge sources."

+++

![jpg](/img/uie.jpg)

## Text2Structure - Structure Extraction Language

```
(
    (Spot Name: Info Span
        (Asso Name: Info Span)
        (Asso Name: Info Span)
    )
)
# Structure extrating language (SEL) for Universal IE
```

* `Spot Name` represents there is a specific information piece with the type of spot name existing in the source text.
* `Asso Name` indicates there exists a specific information piece in the source text that is with the AssoName association to its upper-level Spotted information in the structure.
* `Info Span` represents the text span corresponding to the specific spotting or associating information piece in the source text.


Following is an example:
```
(
    (person: Steve
        (work for: Apple)
    )
    (start-position: became
        (employee: Steve)
        (employer: Apple)
        (title: CEO)
        (time: 1997)
    )
    (orgnization: Apple)
    (time: 1997)
)
# The SEL representation for "Steve became CEO of Apple in 1997."
```

## Prompt paradigm

> feature engineering -> neural network architechure engineering -> fine tuning -> prompt engineering

### How to choose prompt

Different prompts have different zero-shot or few-shot capbilities. For example, the prompt for extrating person name could be:

1. Which people are contained in the original text?
2. Who are in the text?
3. What are the names?

UIE对Prompt的统一：UIE通过大量数据训练固定了Prompt的构造方式，就是`条件+抽取标签`，省去了传统Prompt选择太多的问题。

Prompt和原文越相似，效果越好。

## Conclusion
UIE可以统一建模不同信息抽取任务，按需自适应地生成目标抽取结构，并从不同的知识来源统一学习通用信息抽取能力。

## References

1. Yaojie Lu, etc, from CAS, Baidu and BAAI. [Unified Structure Generation for Universal Information Extraction.](https://arxiv.org/pdf/2203.12277.pdf), ACL 2022.
2. PaddleNLP - UIE. [通用信息抽取 UIE(Universal Information Extraction)](https://github.com/PaddlePaddle/PaddleNLP/tree/develop/model_zoo/uie)
3. [通用信息抽取技术与产业应用实战](https://www.bilibili.com/video/BV1Q34y1E7SW/), 2022.5.21
4. [《UIE：基于统一结构生成的通用信息抽取》-韩先培](https://www.bilibili.com/video/BV19g411Z7rZ/), 2022.7.22
5. https://github.com/universal-ie/UIE