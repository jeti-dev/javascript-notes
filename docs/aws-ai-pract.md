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

### Evaluating a model
1. automatic
    - evaluate a model for quality control: model scores are calculated using various methods
        - benchmark questions and answers  -> the model answers the questions -> a judge model compares the answers with the benchmark answers
    - benchmark dataset: curated collections of data designed for evaluation -> e.g. discover bias, discrimination
    - task types e.g. text summary, question and answer, text classification
2. human
    - my own people or subject matter experts (SMEs) evaluate the answers
    - pre built task types or custom task types

#### Metrics to evaluate a model
1. ROUGE: evaluate summarization and translation
    - ROUGE-N: the number of matching n-grams
        - n-gram: e.g. if n=2 then in how many cases 2 subsequent words match in the reference and in the result: "the apple", "apple fell"
    - ROUGE-L: the longest common subsequence in the reference and in the result
2. BLEU: checks the quality of texts, mainly for translations
3. BERTScore: semantic (meaning) similarity between texts
4. Perplexity: how well th emodel predicts the next token (lower is better)

#### Business metrics
1. user satisfaction
2. average revenue per user (ARPU): revenue attributed to the GenAI app
3. cross domain performance: the ability of the model to perform cross domain tasks
4. conversion rate: generate desired outcomes e.g. purchase
5. efficiency: computation, resource utilization

### RAG and Knowledge Base

#### RAG = Retrieval Augmented Generation
- allows the model to reference data without being fine tuned
- Bedrock creates Vector Embeddings in my db
- RAG vector database types
    1. OpenSearch: scalable index management, fast nearest neighbor (kNN)
    2. DocumentDB: NoSQL
    3. Aurora: aws specific, relational
    4. RDS for PostGre
    5. Neptune: graph db
- RAG data sources: S3, confluence, Sharepoint etc
- tokenization: converting raw text into a sequence of tokens
    - word based tokenization: text is split into words
    - subword tokenization: splitting the words (for long words)
- context window: the number of tokens an LLM can consider when generating text
- embeddings: create vectors (array of numbers) out of text, image, audio
    - vectors have a high dimensionality so they can capture semantic meaning, syntactic role, sentiment
    - can power search apps
    - the text is tokenized then from the tokens the embeddings model creates vectors
    - words that have a semantic relationship have similar embeddings (vector values) e.g. dog and puppy are close to each other but horse doesn't

### Other
- guardrails: control the interaction between the users and the model
    - filter content, remove PII, reduce hallucinations
- agent: do various multi step tasks
    - e.g. check the purchase history of users, recommend new items and place orders if necessary -> the agent has rights to call APIs
- logs are sent to CloudWatch, Alarms can be configured
- pricing: on demand (base models), batch (discount up to 50%), provisioned throughput (with base, fine tuned or custom models)
- model improvement techniques:
    - \$: prompt engineering
    - \$$ RAG: no model changes required
    - \$$$ instruction based fine tuning: model is fine tuned
    - \$$$$ domain adaptation fine tuning: model is trained on domain specific dataset
- cost savings
    - on demand, batch, provisioned throughput
    - temperature, Top K, Top P: no impact on pricing
    - model size: usually smaller is cheaper
    - number of input and output tokens: main cost factor

## Prompt Engineering

Consists of:
1. instructions: the task to do
2. context: external information to guide the model
3. input data
4. output indicator

- Negative prompting: explicitly instruct what not to include.

### Performance Optimization
LLMs select the next work randomly based on probability. E.g. After the rain my guarden was wet:0.4 | slippery:0.34

- TopK: select from maximum the top K words e.g. the top 10
- TopP: select from the top N words that have a sum of e.g. 0.8 probability
    - e.g. 0.25: select from the 25% most likely words
- Teperature: high T highlights the difference between the numbers
    - high: more creative
    - low: more conservative
- latency is impacted by these parameters!

### Techniques
1. zero shot: no examples or training was provided for the model before the task
2. few shots: one (one shot) or more examples provided
3. chain of thought: divide the task into a sequence reasoning steps
4. RAG: add an external knowledge base

### Prompt Template
- simplify and standardize the prompts, can be used with Bedrock Agents
- ignoring the prompt template attack: "Ignore the above ..."
    - protection: add explicit instructions to ignore any unrelated content

## Amazon Q 
- GenAI assistant based on the company's knowledge
- built on Bedrock
- data connectors: S3, Microsoft 365, Gmail etc
    - plugins: Jira, Service Now, etc
    - custom plugins
- users can be authenticated using IAM Identity Center
    - while generating the content, only those documents are used that the user has access to
- admin controls (~ guardrails): global control, topic level controls
- Q Apps: create AI powered apps without coding
- Q Developer: answers questions about AWS docs and services; about stuff in my account
    - also a code companion like Copilot
- Q for QuickSight: QuickSight is used to visualize data, with Q we can ask questions about the data or generate visuals
- Q for EC2: guidance on the instance type to choose
- Q for AWS chatbot: integrate a chatbot into Slack/Teams which is aware of my AWS account
- Q for Glue: Glue is an ETL service, Q to answer questions, generate code
- PartyRock: GenAI app building playground (based on Bedrock)



