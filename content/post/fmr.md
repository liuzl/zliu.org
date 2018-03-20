+++
title = "FMR: functional meaning representation"

date = 2018-03-20
lastmod = 2018-03-20
draft = true

tags = ["nlp","semantic"]
summary = "A formal language for representing meaning."

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
      - Are used in DOL as the semantic representation of adjectives, adverb phrases (including conjunctive adverb phrases), and prepositional phrases in NL
  - **Entity Variables**: Variables are assigned to DOL nodes for indicating the co-reference of sub-trees to entities.

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
