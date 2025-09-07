---
title: AWS AI Practicioner
layout: default
---

# AWS AI Practicioner

Artificial Intelligence > Machine Learning > Deep learning > Generative AI

## Generative AI
- GenAI: generate new data that is similar to the data it was trained on e.g. text, image, audio, video, code
- **foundation model**: we have to rely on them, they are trained on a wide variety of input data
    - text generation, text summarization, information extraction, image generation, chatbot, question answering etc
- **large language models (LLM)** rely on foundational models, generate human-like-text
    - **non-deterministic**: the generated text might be different for the same prompt
    - the LLM creates a list of potential words with probabilities and an algorithm selects a rod from this list
- GenAI for images from text = **diffusion models**
    - training: add noise over time
    - generation: remove noise over time

## Bedrock
- main service of GenAI
- fully managed service
- pay per use
- many foundation models
- my data is not used to train the model
- the foundational model is copied for me, I can customize it
- Amazon Titan: foundation model

### Fine tuning
- the data must be in a specific format and must be stored in S3
- we must use provisioned throughput
- not all models can be fine tuned
- re-training is expensive, instruction based fine tuning is cheaper
    - running a fine tuned model is also more expensive, we must use provisioned throughput

1. instruction based
    - uses labeled examples that are prompt + response pairs: What is Bob's favourite dish? Pancake.
    - A, single turn messaging; needs a special format:
        - system: context for the conversation
        - messages: an array os message objects:
            - role: user or assistant
            - content: the message
    - B, multi turn messaging: the same format as single turn messaing BUT for a conversation .e.g chatbot 
2. continued pre training (or domain adaptation fine tuning)
    - just feed more data to the model e.g. copy and pase a large text

- transfer learning: the broader concept of reusing a pre trained model, widely used for image classification; fine tuning is a specific kind of transfer learning
- use cases:
    - chatbot with a specific persone, tone etc
    - to use more up to date info than what the model knows
    training with exclusive data e.g. my emails
    - targeted use case: e.g. more accurate

## Evaluating a model

