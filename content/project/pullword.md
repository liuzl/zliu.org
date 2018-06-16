+++
# Date this page was created.
date = "2018-06-16"

# Project title.
title = "pullword: Unsupervised Word Discovery"

# Project summary to display on homepage.
summary = "Pullword would be used in settings where only unlabelled text data is available."

# Optional image to display on homepage (relative to `static/img/` folder).
image_preview = "bubbles.jpg"

# Tags: can be used for filtering projects.
tags = ["NLP", "NLU"]

# Optional external URL for project (replaces project detail page).
external_link = "https://github.com/liuzl/pullword"

# Does the project detail page use math formatting?
math = false

# Optional featured image (relative to `static/img/` folder).
#[header]
#image = "headers/bubbles-wide.jpg"
#caption = "My caption :smile:"

+++

## Introduction

With the growing availability of digitized text data, there is a great need for effective computational tools to automatically extract kownledge from texts.

The Chinese language differs most significantly from alphabet-based languages in not specifying word boundaries, most existing Chinese text-mining methods require a prespecified vocabulary and/or a large relevant training corpus, which may not be available in some applications.

Pullword is developed for word discovering from small/large volumes of unstructured Chinese texts, and propose ways to order discovered words and conduct higher-level context analyses and it is particularly useful for mining online and domain-specific texts where the underlying vocabulary is unknown or the texts of interest differ significantly from available training corpora.

## Implementations

The implementations mainly follow this [post](http://www.matrix67.com/blog/archives/5044).

* [golang version](https://github.com/liuzl/pullword)
* [javascript version](https://github.com/nlpclub/nlpclub.github.io/blob/master/pullword/js/pullword.js)

## Online demo

* [javascript demo](https://nlpclub.github.io/pullword/)


