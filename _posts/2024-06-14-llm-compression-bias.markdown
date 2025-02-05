---
layout: post
title:  "How does compression of Large Language Models affect their bias?"
description: Reflections on a project exploring the relationship between model compression and bias
date:   2024-06-14 12:00:00 
categories: llms
---

**14th June 2024**

*In the last 18 months, large language models (LLMs) have sprung into public consciousness. Now, more than ever, you can read about how AI is so advanced it will [take over our jobs](https://www.bbc.com/news/technology-65102150) but also problems with the models including [political](https://www.washingtonpost.com/technology/2023/08/16/chatgpt-ai-political-bias-research/) or [racial](https://www.scientificamerican.com/article/even-chatgpt-says-chatgpt-is-racially-biased/) biases. Whilst these models have proven to be useful tools, there clearly remains a way to go for improving how we work with them. I recently worked on a [project](https://github.com/abigailhayes/LLM-Pruning-And-Fairness) that looked at tackling two of the issues identified with LLMs: the demands or their increasing size and the bias they demonstrate. Over a series of posts, I will now explore some of the themes involved. This is the second post in the series (see [Part 1 - Do LLMs have to be so big?]({% post_url 2024-04-13-llm-size %}, [Part 2 - How can we make LLMs smaller?]({% post_url 2024-05-04-llm-compression %} and [Part 3 - How can we measure the bias of LLMs?]({% post_url 2024-05-14-llm-bias %})).*

The question of how model compression interacts with bias was at the core of our research project, but we began by looking at existing work which has looked at similar questions.

## What does published research show?

Multiple papers that explore this interaction between model compression and bias have been published, but the results are still inconclusive. There are two initial theories: compressing a model reduces bias by removing parts which encode the bias or compressing a model forces it all the more to rely more heavily on biases to make predictions.

[Xu and Hu](https://arxiv.org/abs/2201.08542) apply two different compression methods (knowledge distillation and structured attention head pruning) to a GPT2 model and find that this consistently reduces bias and toxicity. However, they are not confident about whether the bias measures of StereoSet and the WinoBias datasets actually measure the abstract concept because of their small dataset size.

[Hessenthaler et al.](http://arxiv.org/abs/2211.04256) and [Ahn et al.](https://aclanthology.org/2022.gebnlp-1.27) also study the interaction of knowledge distillation and fairness, but they find the opposite effect. When measuring bias with a range of measures it generally increases when the model size is reduced.

[Ramesh et al.](https://aclanthology.org/2023.acl-long.878) explore a range of models and model compression methods (pruning in pre-training, distillation and quantisation), again with multiple bias measures. They find pruning and distillation generally amplify bias whilst quantisation reduces it, but only as the model also performs worse at the original task. 

## What did our project find?

Our project was developed on a RoBERTa model and we apply various pruning methods for model compression. We also evaluate bias with multiple measures because of potential issues with single measures identified previously. We explore different sizes of models and the bias they display.

Overall, there are no clear conclusions in the question of model compression vs bias. This is largely due to the frequent disagreement between the bias measures. Where one measure indicates a decrease in bias, another may indicate an increase. The bias measures do not agree with one another, which raises a question of whether or not they are actually able to measure bias at all. A major conclusion from our project is that more work should be done on developing a rigorous bias measure with theoretical grounding.

When we consider only the extrinsic bias measures, which should indicate the bias a model would demonstrate when actually in use, as models are compressed the bias decreases. However, this corresponds to a concurrent decrease in model performance on the actual task. We conclude that this is due to many bias measures rewarding randomness. A poorly performing model is generally choosing randomly as it has not learnt enough information for correct choices. Similarly, if a model randomly chooses between different bias categories e.g. male and female, a model can appear unbiased. This is a secondary problem that would need to be tackled if developing a future bias measure.

Whilst the project may not have given any clear answers to the original question, the importance of such work is not diminished. There remains clear benefits from considering model desirability in a more rounded way than simply how well it completes a task. Hopefully future research will be able to better measure model bias and facilitate a future where we can have more confidence in the LLMs we want to use.
