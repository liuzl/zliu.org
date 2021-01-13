+++
title = "Language Model Is All You Need — Explores NLU as MRC QA"

date = 2021-01-12
lastmod = 2021-01-12
draft = false

tags = ["NLP", "NLU", "QA", "Language Model", "QANLU"]
summary = "One Amazon Alexa AI's new paper *Language Model Is All You Need* explores NLU problem as a QA problem. The original paper is on [arXiv](https://arxiv.org/abs/2011.03023)."

+++

New research from Amazon Alexa AI posits that current natural language understanding (NLU) approaches are far from how humans understand language, and asks whether all NLU problems could be efficiently and effectively mapped to question-answering (QA) problems using transfer learning.

Transfer learning is an ML approach for applying knowledge learned from a source domain to a target domain. It has produced promising results in natural language processing (NLP), particularly when transferring learning from high data domains to low data domains. The Amazon researchers focus on a specific type of transfer learning, where the target domain is first mapped to the source domain.

![png](/img/qanlu_fg1.png)

NLU is taken as determining intent and slot or entity value in natural language utterances. The proposed "QANLU" approach builds slot and intent detection questions and answers based on NLU annotated data. QA models are first trained on QA corpora the fine-tuned on questions and answers created from the NLU annotated data. Through transfer learning, this contextual question-answering knowledage is then used for finding intents or slot values in text inputs.

Unlike previous approaches, QANLU focuses on how low resource applications and does not the design and training of new model architectures or extensive data preprocessing. This enables it to achieve strong results in slot and intent detection with an order of magnitude less data.

![png](/img/qanlu_fg2.png)

The researchers conducted experiments on the ATIS and Restaurants-8k datasets, with QANLU in low data regimes and few-shot settings significantly outperforming sentence classification and token tagging approaches for intent and slot detection tasks, while also bettering the new IC/SF few-shot approach's performance in NLU.

The researchers say future directions could include expanding beyond this configuration and across different NLP problems, measuring the transfer of knowledge across different NLP tasks, and studying how QANLU questions might be generated automatically based on context.

The paper *Language Model Is All You Need: Natural Language Understanding as Question Answering* is on [arXiv](https://arxiv.org/pdf/2011.03023.pdf).


