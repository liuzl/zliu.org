+++
# Date this page was created.
date = "2018-02-16"

# Project title.
title = "FMR: functional meaning representation"

# Project summary to display on homepage.
summary = "A formal language for representing meaning and a system for semantic parsing."

# Optional image to display on homepage (relative to `static/img/` folder).
image_preview = "bubbles.jpg"

# Tags: can be used for filtering projects.
# Example: `tags = ["machine-learning", "deep-learning"]`
tags = ["nlp", "nlu", "semantic parsing"]

# Optional external URL for project (replaces project detail page).
external_link = "https://github.com/liuzl/fmr"

# Does the project detail page use math formatting?
math = false

# Optional featured image (relative to `static/img/` folder).
#[header]
#image = "headers/bubbles-wide.jpg"
#caption = "My caption :smile:"

+++

## Element types in FMR

  - **Constants**: Refer to specific objects in the world. A constant can be a number, a lexical string, or an entity.
  - **Classes**: Semantic category of entities. For example: `location.city`, `math.number`
    - Sub-class: *math.number ⊆ math.expression*
    - Template classes: classes with one or more parameters, for example: `t.list<c, m, n>`, where `c` is a class, `m` and `n` are integers.
  - **Functions**: The major way to form larger language units from smaller ones. A function is comprised of a *name*, a list of *core arguments*, and a *return type*.
    - Noun functions
      - Map entities to their properties or to other entities having specific relations withe the argument(s).
      - Are used to represent noun phrases in natural language.
      - Pronoun functions are special zero-argument noun functions.
    - Verb functions
      - Act as sentences or sub-sentences
      - The most important function type?
    - Modifier functions
      - Many functions can take additional *extended arguments* as their modifiers.
      - Modifier functions often take the role of extended arguments to modify noun function, verb functions or other modifier functions.
      - Are used in FMR as the semantic representation of adjectives, adverb phrases (including conjunctive adverb phrases), and prepositional phrases in NL
  - **Entity Variables**: Variables are assigned to FMR nodes for indicating the co-reference of sub-trees to entities.

## Features of FMR

  - Strongly typed language
    - Type-compatibility: The type of each child of a function node should match the corresponding argument type of the function.
  - Open-domain type system.
    - Thousands of types
    - Other languages: At most 100+ in grammar level
  - Built-in data structures
    - like `t.list` and `nf.list`

## Semantic parsing from NL to FMR

By using CFG rules to map natural language sentences and phrases to FMR
```javascript
"The product of 3 consecutive number is 60" => 
vf.be.equ(nf.math.product(nf.list(math.number, 3, mf.consecutive)), 60);
```

## 缘起

* 语义理解平台/语音助手（2013-今，腾讯/搜狗）
  * 复合实体识别（CRF识别/规则识别/词表识别）
  * 基于语言模型的查询意图理解
  * **基于语义模板的查询意图识别和语义信息抽取**
      * 通用CFG文法解析器：高效Earley算法的实现
      * 意图识别，槽位填充
  * 查询意图排序，基于候选意图结果的终判模块
  * 下游分类别知识库系统/垂直搜索系统
