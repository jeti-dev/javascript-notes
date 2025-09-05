---
title: AWS AI Practicioner
layout: default
---

# AWS AI Practicioner

Artificial Intelligence > Machine Learning > Deep learning > Generative AI

## Generative AI
- genAI: generate new data that is similar to the data it was trained on e.g. text, image, audio, video, code
- **foundation model**: we have to rely on them, they are trained on a wide variety of input data
    - text generation, text summarization, information extraction, image generation, chatbot, question answering etc
- **large language models (LLM)** rely on foundational models, generate human-like-text
    - **non-deterministic**: the generated text might be different for the same prompt
    - the LLM creates a list of potential words with probabilities and an algorithm selects a rod from this list
- genAI for images from text = **diffusion models**
    - training: add noise over time
    - generation: remove noise over time

## Bedrock
- main service of genAI
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
