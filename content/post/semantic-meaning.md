+++
title = "⛵ Learning Meaning in Natural Language Processing — The Semantics Mega-Thread"

date = 2018-11-15
lastmod = 2018-11-15
draft = false

tags = ["NLP", "NLU", "Semantic", "Meaning"]
summary = "I found a very worthwhile article while surfing medium.com days ago. The article is a summary of a twitter thread which talked about meaning, semantics, language models, learning Thai and Java, entailment, co-reference — all in one fascinating thread. The original article is [here](https://medium.com/huggingface/learning-meaning-in-natural-language-processing-the-semantics-mega-thread-9c0332dfe28e)."

+++

I found a very worthwhile article while surfing medium.com days ago. The article is a summary of a twitter thread which talked about meaning, semantics, language models, learning Thai and Java, entailment, co-reference — all in one fascinating thread. The original article is [here](https://medium.com/huggingface/learning-meaning-in-natural-language-processing-the-semantics-mega-thread-9c0332dfe28e).

Following is a copy of the original article:

Last week [a tweet by Jacob Andreas](https://twitter.com/jacobandreas/status/1023246560082063366) triggered a huge discussion on Twitter that many people have called the *meaning/semantics mega-thread*.

## 🏎 A crash course on Lexical Meaning & Semantics

If you already know what we mean by *“Meaning/Semantics”* in NLP, you can skip this part and go straight to the debate 🔥.

For the CS/ML folks out there here are a few words of introduction.

First, it’s important to state that Meaning in Natural Language is a multi-facetted concept with semantic, pragmatic, cognitive and social aspects. The discussion that happened on Twitter was mainly about lexical semantics and compositionality so I will focus on this sub-field for brevity. You will find additional links to broaden this view at the end of this section.

> [Meaning](https://en.wikipedia.org/wiki/Meaning_%28linguistics%29) is the information that a sender intends to convey, or does convey, to a receiver.

Now, we know that **strings are already a representation of meaning**, so why should we go any further than just raw text?

Well there are several reasons we may want to distinguish meaning from raw text.

One reason is that the field of NLP/NLU aims at **building systems** that understand what you say to them, trigger actions based on that and convey back meaningful information. Let’s take a simple example:

```
Context: Knowledge of mathematics
Utterance: What is the largest prime less than 10?
Action: 7
```

Given some knowledge of math, we want our NLU system to produce an appropriate answer.

It’s difficult to (i) link raw text to a knowledge base of mathematical facts in our system and (ii) combine pieces of knowledge together to infer an answer. One solution is to define an intermediate meaning representation (sometimes called a Logical Form) that is more easy to manipulate.

For example in our case:

$$
Meaning Representation = max(primes \cap (-\infty, 10))
$$

We can then execute this expression with respect to a model of the world, like our database of knowledge, to get an answer. This way, we have also factored out the understanding of language (called *semantic parsing*) from the world knowledge (the problem of grounding meaning of in the real word).

Advantageously, our *representation of the meaning of a sentence* can thus:

1. provide a way to **link language** to external **knowledge base**, **observations**, and **actions**;
2. support **computational inference**, so that concepts can be **combined** to derive additional knowledge as human do during a conversation.

Two other nice requirements for this representation:

1. **unambiguous**: one meaning per statement (unlike natural language);
2. **expressive** enough to cover the full range of things that people talk about.

Natural language as raw text doesn’t fulfill most of these criteria!

A related line of research is **Formal Semantics** which seek to understand linguistic meaning by constructing models of the principles that speakers use to convey meaning.

The tools of formal semantics are similar to NLU/NLP tools but the aim is to understand how people construct meaning more than any specific application.

Now there is a lot more to meaning than just logic forms and grounding. A few examples: **“But I didn’t mean it literally!!”** (speaker meaning ≠ literal meaning), **“Buffalo buffalo Buffalo buffalo buffalo buffalo Buffalo buffalo.”** (yes, this is a real sentence with a meaning but you need to find the right sense for each buffalo!) and so on...

> A few pointers: Our simple example came from [this nice article by Percy Liang](https://cs.stanford.edu/~pliang/papers/executable-cacm2016.pdf). As a quick overview of the field, I would recommend chapters 12 and 13 of J. Eisenstein’s book [“Natural Language Processing”](https://github.com/jacobeisenstein/gt-nlp-class/blob/master/notes/eisenstein-nlp-notes.pdf). They will take you through the main ideas, tools up to recent research on Meaning in NLP. [Emily M. Bender’s ACL 2018 tutorial](http://faculty.washington.edu/ebender/100things-sem_prag.html) is a nice way to see how Meaning can be a multi-headed monster 🐍 to say the least!

Now back to our mega-thread!

## 🔥 Triggering a debate on Meaning

As often, the discussion was sparked by a mention of sentence embeddings.

This argument was the main trigger for the mega-discussion that followed. Further in the thread, Emily M. Bender reformulates her argument as:

> If all the learner gets is text the learner cannot learn meaning.

## Can a model trained only on raw text learn meaning?

This question was explored along two axes:

### What aspect of Meaning can a model learn?

* Can it learn *meaning* or just learn *similarity* in meaning (i.e. learn that some expressions are similar without knowing what they mean. It is still very useful for transfer learning)?

* Can it learn *grounded meaning* (learn the meaning of each expression as a state of the world state) or learn *lexical meaning* (e.g. learn how the meaning of sub-expressions compose together, as in our logical forms)?

### How can the model Learn?

* If the model cannot learn meaning from raw text alone, what would be the *minimal amount of additional supervision* needed? Should we add supervision from Logical Forms, Textual Entailment...?

* Could we encode some *inductive bias* in the model so that it can learn aspects of meaning from raw text?

## The Thai and Java Experiments

Emily M. Bender proposed several interesting experiments which were discussed at length:

* [The Thai Room experiment](https://twitter.com/emilymbender/status/1024042044035985408): *“Imagine [you] were given the sum total of all Thai literature in a huge library. (All in Thai, no translations.) Assuming you don’t already know Thai, you won’t learn it from that.”* **A real life example of trying to learn from raw text only.**
* [The Java Code experiment](https://twitter.com/emilymbender/status/1025002835467890689): *“Give your NN all well-formed java code that’s ever been written, but only the surface form of the code. Then ask it to evaluate (i.e. execute) part of it.”* **Can we learn execution semantics from raw text only?**

## Investigating Programming Language Semantic

The Java Code proposal triggered an interesting discussion on the difference between trying to learn meaning from Programming Language (PL) code and from Natural Language (NL) text.

It actually seems more difficult to learn from PL than NL.

Learning meaning from Java code is like having a text composed only of orders/commands and without any descriptions. But description are very important feedbacks for learning as they allow one to compare its internal world state with the real world state.

## Language Models

The discussion circled around Language Models. A language model is a model which can **predict the next word in a sentence given a context** (past words, external knowledge). Recently, these models have given [interesting results](https://blog.openai.com/language-unsupervised/) in commonsense reasoning. Language models were examined in two settings:

* **Independently of any training dataset**: having a *human-level language model* involves that the model has a human-like notion of meaning and world state. As [Yolav Goldberg mentioned](https://twitter.com/yoavgo/status/1025485692347056128) *“it’s just (one of the many) trivial examples where perfect LM entails solving not only all of language but also all of AI.”*

* More interesting is the case of a language model **trained from raw text only**: here the question is how much the model can learn in terms of semantics without being given access to explicit meaning information!

## Textual Entailment

One way to give some information about meaning without departing too much from raw text is to train a model on Textual Entailment, the task of **predicting whether a sentence imply another**.

In a series tweets, Sam Bowman explained his view that entailment could be used to learn “the meat of compositional” semantics almost from raw text.

And also made the suggestion that a learner might be able to learn entailment in a simpler way than curent setups, using a setup that could be close to LM.

## An Inductive Bias Language Model

Another way to learn meaning from a dataset as close as possible to raw text is to put a strong inductive bias in the model as discussed by Matt Gardner.

One example is the [Entity Language Model](https://arxiv.org/abs/1708.00781) which augments a classical LSTM by explicitly modeling an arbitrary number of entities, updating their representations along the hidden state, and using the mention representations to contextually generate the next word in the LM task.

> To read more about that, check [Yejin Choi’s talk at ACL 2018](https://sites.google.com/site/repl4nlp2018/) and [Percy Liang’s talk at AAAI 2018](https://www.youtube.com/watch?v=7CcSm0PAr-Y).

## The Big Open Question

In the end, I feel like the main original question stayed open: can a model learn some aspects of lexical meaning from raw text alone?

## A word on Searle’s Chinese Room

[Searle’s room argument](https://en.wikipedia.org/wiki/Chinese_room) came back often in the discussion but the situation was a bit different.

Searle’s argument was made in the Strong versus Weak AI debate: does the computer has a mind or consciousness. Here the question is less philosophical: can we extract a representation of meaning from form alone.

Still, [as Jeremy Howard detailed a bit later](https://twitter.com/jeremyphoward/status/1026881686359822337), the Chinese room experiment of Searle goes far beyond the question of strong/weak AI to the question of understanding/qualia, so please go [check this thread](https://twitter.com/jeremyphoward/status/1026881686359822337).