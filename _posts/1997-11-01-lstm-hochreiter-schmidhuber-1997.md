---
layout: post
title: "The Memory That Saved Neural Networks"
subtitle: "Two researchers in Munich published a paper solving the vanishing gradient problem, quietly laying the foundation for every modern AI that understands sequences."
date: 1997-11-01
event_date: "1997-11-01"
era: "Statistical Learning and Quiet Progress (1993–2010)"
category: "Research"
location: "Munich, Germany"
people:
  - "Sepp Hochreiter"
  - "Jürgen Schmidhuber"
quote: "The problem is that backpropagated error signals either shrink or grow exponentially. This is the fundamental deep learning problem."
quote_author: "Sepp Hochreiter"
significance: "Long Short-Term Memory networks solved the critical problem of learning long-range dependencies in sequential data. Ignored for over a decade, LSTMs would eventually power Google Translate, Siri's speech recognition, and countless other applications — becoming the dominant architecture for sequence modeling until transformers arrived in 2017."
image: "/assets/images/lstm-hochreiter-schmidhuber-1997.jpg"
image_alt: "The Memory That Saved Neural Networks"
image_caption: "SeppHochreiter / CC BY-SA 4.0"
image_source: "https://commons.wikimedia.org/wiki/File%3ASepp_Hochreiter_4.jpg"
references:
  - title: "Long short-term memory - Wikipedia"
    url: "https://en.wikipedia.org/wiki/Long_short-term_memory"
  - title: "Hochreiter & Schmidhuber (1997) - Neural Computation"
    url: "https://doi.org/10.1162/neco.1997.9.8.1735"
  - title: "The Deep Arbitrary Polynomial Chaos Neural Network or how Deep Artificial Neural Networks could benefit from Data-Driven Homogeneous Chaos Theory (arXiv)"
    url: "http://arxiv.org/abs/2306.14753v1"
---

# The Memory That Saved Neural Networks (1997)

In 1997, Sepp Hochreiter and Jürgen Schmidhuber introduced Long Short-Term Memory (LSTM) networks, a type of recurrent neural network designed to address the vanishing gradient problem. LSTM units are composed of a cell and three gates: input, output, and forget gates, which regulate the flow of information and allow the network to remember information for long periods. This innovation was initially overlooked but eventually became crucial for applications like Google Translate and Siri's speech recognition.

**Why it matters:** The introduction of LSTMs in 1997 marked a significant advancement in artificial intelligence, particularly in handling sequential data with long-range dependencies. Despite being ignored for over a decade, LSTMs revolutionized sequence modeling and dominated the field until the advent of transformers in 2017.

**Further reading:**
- [Long short-term memory - Wikipedia](https://en.wikipedia.org/wiki/Long_short-term_memory)
