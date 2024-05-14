---
layout: post
title:  "How can we make Large Language Models smaller?"
description: Reflections on a project exploring the relationship between model compression and bias
date:   2024-05-04 12:00:00 
categories: llms
---

**4th May 2024**

*In the last 18 months, large language models (LLMs) have sprung into public consciousness. Now, more than ever, you can read about how AI is so advanced it will [take over our jobs](https://www.bbc.com/news/technology-65102150) but also problems with the models including [political](https://www.washingtonpost.com/technology/2023/08/16/chatgpt-ai-political-bias-research/) or [racial](https://www.scientificamerican.com/article/even-chatgpt-says-chatgpt-is-racially-biased/) biases. Whilst these models have proven to be useful tools, there clearly remains a way to go for improving how we work with them. I recently worked on a [project](https://github.com/abigailhayes/LLM-Pruning-And-Fairness) that looked at tackling two of the issues identified with LLMs: the demands or their increasing size and the bias they demonstrate. Over a series of posts, I will now explore some of the themes involved. This is the second post in the series (see [Part 1 - Do LLMs have to be so big?]({% post_url 2024-04-13-llm-size %})).*

## How can an LLM be compressed?

In my last post, I looked at the motivation behind wanting smaller LLMs. Now comes the practical question of how to actually achieve that. It is not just a case of wildly removing parts, but instead trying to develop methods which still result in a useful model. [Zhu et al.](http://arxiv.org/abs/2308.07633) take a much more in depth look into the topic.

### 1. Quantisation

During quantisation, floating point numbers are replaced with integers during or after training. This results in a lower demand for storage space with an unchanged number of parameters. The idea behind this being that the weights are only changed a small amount, and so it is hoped that the model still provides a similar output.

### 2. Knowledge distillation

For knowledge distillation, model size is reduced by using a smaller child model, which learns from the original parent model. This means that the desired final size can be pre-determined.

### 3. Pruning

Pruning is the process of completely removing parameters that are judged unimportant from the model. The targeted parameters could be removed individually (unstructured pruning), or by removing whole layers or components from the model (structured pruning). This method can be easily adapted to experiment with different sizes of the final model, without much additional effort. However, it requires more theoretical background to decide which aspects of the model should be pruned, rather than applying a single technique to the whole.

## How much should an LLM be compressed?

Generally, the extent of model compression is determined by model performance. Whilst it is desirable to have a smaller model, if it can no longer make good predictions then it is unusable. Whilst it might be expected that removing parts of a model will degrade performance, they have nonetheless shown surprising resilience in research ([Michel et al.](https://proceedings.neurips.cc/paper_files/paper/2019/hash/2c601ad9d2ff9bc8b282670cdd54f69f-Abstract.html), [Prasanna et al.](https://www.aclweb.org/anthology/2020.emnlp-main.259))

In practice, model performance is evaluated as the compression technique is applied, until the "right" compromise between size and performance is found. But is that really all we should care about? Perhaps the model is still able to provide acceptable outputs, but do we need to care more about how it is getting to them? A growing body of research is concerned with other aspects of models, including the biases they display. It remains an open question as to whether model compression amplifies or reduces biases. And beyond that, it remains at the judgement of the researcher as to whether potential changes in bias should also factor into the extent of model compression.

The project I worked on examined how the biases of models change as the models are pruned. In the next blog I will begin to explore the basis of our experiments, by looking at how model bias might be measured.