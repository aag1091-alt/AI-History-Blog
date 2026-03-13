---
layout: post
title: "King Minus Man Plus Woman Equals Queen"
subtitle: "A deceptively simple algorithm revealed that words have geometry, launching the era of word embeddings and transforming how machines understand language."
date: 2013-01-16
event_date: "2013-01-16"
era: "The Deep Learning Revolution (2011–2019)"
category: "Research"
location: "Mountain View, United States"
people:
  - "Tomas Mikolov"
  - "Kai Chen"
  - "Greg Corrado"
  - "Jeffrey Dean"
quote: "The word vectors capture many linguistic regularities, for example vector('King') - vector('Man') + vector('Woman') results in a vector very close to vector('Queen')."
quote_author: "Tomas Mikolov"
significance: "Word2Vec demonstrated that neural networks trained on raw text could learn vector representations where arithmetic on words produced semantically meaningful results. This insight — that meaning could be encoded as direction in space — became foundational to every major language model that followed, from GloVe to BERT to GPT."
image: "/assets/images/word2vec-mikolov-2013.png"
image_alt: "King Minus Man Plus Woman Equals Queen"
image_caption: "Fan Gao, Hang Jiang, Rui Yang, Qingcheng Zeng, Jinghui Lu, Moritz Blum, Dairui Liu, Tianwei She, Yuang Jiang, Irene Li / CC0"
image_source: "https://commons.wikimedia.org/wiki/File%3AThe_three_main_prompt_types_we_compared_%28Figure_1_from_Gao_et_al.%2C_Evaluating_Large_Language_Models_on_Wikipedia-Style_Survey_Generation%29.png"
references:
  - title: "Word2vec - Wikipedia"
    url: "https://en.wikipedia.org/wiki/Word2vec"
  - title: "Efficient Estimation of Word Representations in Vector Space (original paper)"
    url: "https://arxiv.org/abs/1301.3781"
  - title: "On the Jones Polynomial (arXiv)"
    url: "http://arxiv.org/abs/2108.13835v2"
---

# The Birth of Word2vec: A Milestone in AI

In 2013, a groundbreaking technique called Word2vec emerged from Google, revolutionizing the field of natural language processing.

**What happened:** In 2013, Tomáš Mikolov, Kai Chen, Greg Corrado, and Jeffrey Dean at Google introduced Word2vec, a method for generating vector representations of words based on their context in large text corpora. This technique allowed for the detection of synonymous words and the suggestion of additional words for partial sentences, marking a significant advancement in understanding and processing language. [Word2vec - Wikipedia](https://en.wikipedia.org/wiki/Word2vec)

**Why it matters:** Word2vec demonstrated that neural networks trained on raw text could learn vector representations where arithmetic on words produced semantically meaningful results. For example, the vector equation "king - man + woman = queen" showed that relationships between words could be captured in the direction of vectors in high-dimensional space. This insight became foundational to subsequent language models like GloVe, BERT, and GPT, shaping the trajectory of AI research in natural language processing.

**Further reading:**
- [Efficient Estimation of Word Representations in Vector Space](https://arxiv.org/abs/1301.3781)
