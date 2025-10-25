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

## AI and Machine Learning Overview
- Artificial Intelligence > Machine Learning > Deep Learning > Generateive AI
- AI components:
    1. data layer
    2. ML famework and algorithm layer
    3. model layer: implement the model, train it, set parameters, functions, optimizer functions
    4. application layer: how to serve the model and its capabilities

### Machine learning
Methods that allow machines to learn.

### Deep learning
- use neurons and synapses to train a model.
- deep: more than 1 layer of learning e.g. object detection, image classification; natural language processing (NLP) like text classification
- neural networks: nodes (even billions) are connected together and organized in layers
    - when the neural netwrok sees a lot of data, it identifies patterns and changes the connections between the nodes
    - nodes are communicating with each other by passing or not passing data to the next layer
    - e.g. recognizing hand written numbers: each layer is able to recognize 1 property like 'curved bottom' or 'vertical lines'

### GenAI
- subset of deep learning
- model types:
    1. transofmer model (LLM): able to process a sentence as a whole instead of word by word
    2. diffusion models: when creating an image: making an image more noisy step by step when training and reverse it when generating (from noise to image)
    3. multi-modal models: does not rely on a single type of input and does not create a single type of input
- training data: can be labeled or unlabeled, structured or unstructured
    - labeled: data includes input features and output labels, used for supervised learning
    - unlabeled data: data includes only input features, used for unsupervised learning
    - structure: usually has a table like format
    - unstructured: often text heavy or multimedia
- supervised learning
    - needs labeled data
    - can predict the output for a new unseen input data
    - regression: predict a numberic value based on the input; when we have to predict a quantity or a real value; the output variable is continous as it can take any value within a range
    - classification: predict the categorical label of input data; the output variable is discrete meaning it falls into a speciic category; e.g. the email is spam or not; k-nearest neighbours algorithm (k-NN)
- data types
    - training set: 60-80% of the data set, used to train the model
    - validation set: 10-20%, to tune the model parameters and validate performance
    - test set: 10-20%, to evaluate the final model performance

Feature engineering:
The process to select and transform raw data to meaningful features, useful for supervised learning
- types
    - feature extraction/creation: e.g. derive age from date of birth
    - feature selection: selecting a subset of relevant features e.g. use only the important data in a prediction model
    - feature transformation: transforming data for better model performance e.g. normalize numerical data
- feature engineernig on structured data e.g. predict house prices based on some features
    - creation: deriving features like "price per square foot"
    - selection: identifying and retaining important features e.g. location and number of bedrooms
    - transformation: normalizing features to ensure they are on a similar scale
- feature engineering on unstructured data e.g. sentiment analysis of customer reviews
    - text data: convert text into numerical features (TF-IDF) or word embeddings
    - image data: extract features such as edges or textures using e.g. convulational neural networks (CNNs)

Important terms
- GPT (generative pre trained transformer): generate text or code
- BERT (bidirectional encoder representations from transformers): similar to GPT but reads the text in two directions
- RNN (recurrent neural network): meant for sequential data e.g. time series text, useful in speech recognition and time series prediction
- ResNet (residual network): CNN (deep convolutional network) used for image recognition, detection
- SVM (support vector machine): ML algorithm for classification and regression
- WaveNet: generate audio waveform
- GAN (generative adversarial network): models that help generate data (images, videos, sounds) similar to the training data to help data augmentation
- XGBoost (extreme gradient boosting): an implementation of gradient boosting

### ML Algorithms

#### Unsupervised learning 
- to discover patterns in the input data
- the machine creates the groups but the humans put labels on them
- common techniques: clustering, association rule learning, anomaly detection
1. clustering: group similar data points together into clusters
    - e.g. customer segmentation with K-means Clustering
2. association rule learning
    - e.g. which products are often bought together to help us with promotions
3. anomaly detection: detect fraudulent credit card usage using Isolation FOrest technique

#### Semi-supervised learning
- use a small amount of labeled data and a large amount of unlabeled data
- then the partially trained algorithm itself labels the unlabeled data = pseudo labeling
- lastly the model is re-trained on the resulting data

#### Self-supervised learning
- the model creates pseudo-labels for its own data without humans labeling data first
- widely used in NLP and in image recognition
- pre-text task e.g. let the model itself learn the rotation of images (while also learning about the images) -> but in the end we want the model to know stuff about images and not the rotation

#### Reinforcement learning (RL)
- an agent learns to make decisions by doing actions in an environment while maximizing rewards e.g. finding the door in a maze where every block has a score -> the more score the shorter is the route
- key concepts
    - agent: the learner
    - environment: the external system the agent interacts with
    - actions: the choices the agent makes
    - reward: the feedback from the environment based on the actions
    - state: the current situation of the environment
    - policy: the strategy used by the agent to determine the actions based on the state
- learning process
    - the agent checks the current state of the environment
    - makes an action based on the policy
    - the environment transitions to a new state and gives a reward
    - the agent updates its policy to improve future decisions
    - goal: get max reward

#### Reinforcement Learning from Human Feedback (RLHF)
- use human feedback to help models self learn more efficiently
- the human feedback is added to the rewards function to be more aligned with human goals
- used in GenAI apps including LLM Models
- significantly enchanes the model performance
- e.g. grading transalations from "technically correct" to "human"
- steps to create a company specific chatbot
    1. data collection e.g. questions and answers
    2. supervised fine tuning of a language model: fine tune an existing model with the data
    3. build a separate reward model: humans can set which response they prefer for a given prompt -> the reward model can estimate which response would a human choose
    4. use the reward model as a reward function for the reinforcement learning

### Performance analysis
- model fit:
    - overfit: good performance on the training data, bad on the evaluation data (all dots are connected)
    - underfitting: poor performance on training data (no dots are connected); maybe the model is too simple or the data is not good
    - balanced: what we want (some dots are connected)
- bias and variance:
    - bias: difference or error between the predicted and the acutal value (high bias = underfitting) or in other words it can't learn the pattern; probably thanks to bad ML process choices -> use a more complex model, increase the number of features
    - variance: how much the performance of a model changes if trained on a different set of data which has a similar distribution (high variance = overfitting); use feature selection (focus on more important features)
    - low bias and low variance = balanced model fit
- model evaluation

#### Model evaluation 
#### Confusion matrix
- To evaluate a classification model.
- metrics:
    - precision: best when false positives are costly
    - recall: best when false negatives are costly
    - F1 score: best when we want balance in an imbalanced dataset
    - accuracy: best for balanced datasets
- AUC-ROC: area under the curve-receiver operator curve
 - value is 0-1 (=perfect model)
 - what the curve for true positives compared to false positives looks like at various thresholds -> we can decide which threshold to use

2d matrix example (can be multi dimensional)

|Metric|Calculation|Interpretation|
|------|-----------|--------------|
True Positive (TP)|Correctly predicted positive|Successes in identifying the target class.
True Negative (TN)|Correctly predicted negative|Successes in identifying the non-target class.
False Positive (FP)|Incorrectly predicted positive|Type I Error (False Alarm).
False Negative (FN)|Incorrectly predicted negative|Type II Error (Missed Opportunity).

|Metric|Formula|Focus/Goal|
|------|-------|----------|
|Accuracy|(TP+TN)/(FP+FN+TP+TN)​|Overall correctness. Good for balanced data.|
Precision|TP/(FP+TP)​|Quality of Positive Predictions. Minimizing FP.
|Recall (Sensitivity)|TP/(FN+TP)​|Completeness of Positive Identification. Minimizing FN.|
|F1-Score|2\*Precision\*Recall/(Precision+Recall)​|Harmonic mean of Precision and Recall. Useful for imbalanced data.|

#### Regressions metrics
- for continous data (not for classification!)
- types
    - MAE (mean absolute error): between the predicted and the actual values
    - MAPE (mean absolute percentage error)
    - RMSE (root mean squared error)
    - R2 (R squared): explains variance in the model, close to 1 means good
- MAE, MAPE, RMSE measure the error, how accurate the model is
    - e.g. if RMSE is 3 this means that on average the model predicts a student's score 3 points off
- R2: what percentage of the problem's solution is explained by the variables I included in the model
    - e.g. when trying to predict the final price of a house and I supply the house prices, then if R2 is 85% it means that the square foot is a good predictor. But if R2 is 15% is means that other factors are also important in the house price e.g. location, age

#### Inferencing
- when a model is making prediciton on new data
- can be real time or batch
- inferencing at the edge: mobile, tablet, raspberry etc

#### Phases of an ML project
1. define business problem
2. ML problem framing: convert the business problem to a ML problem
3. data collection and preparation
4. feature engineering
5. model training and paramtere tuning
6. model evaluation: if the result is not good, go back to step 3 or 4
7. model testing and deployment
8. monitoring and debugging
9. add new data and retrain? go to step 3 or 5

#### Hyperparameter tuning
- definition: settings that define the model structure, leaning algorithm and process before the training begins
- e.g.
    - learning rate: how big are the steps when updating the model's weights during training: high can lead to faster convergence but risks overshooting the optimal solution; low causes slower convergence but more precision
    - batch size: number of training examples to use to update the model's weight in one iteration: small lead to more stable learning but require more time to compute
    - number of epochs: how many times the modell will iterate over the entire dataset: too few causes underfitting and too many causes overfitting
    - regluarization: adjusting the balance between simple and complex model: increase to reduce overfitting
- hyperparameter tuning: finding the best hyperparameters to optimize the model performance
- how to do it e.g. use SageMaker Automatic Model Tuning (AMT)
- what to do when overfitting
    - symptom: good predictions on the training data but not good for the new data
    - reason:
        - too small training data, not representing all possible input values
        - the model trains too long on a single sample set of data
        - model complexity is high and learns also the noise in the training data
    - fix:
        - add more training data
        - use less epochs
        - data augmentation (to increase diversity)
        - adjust hyperparameters

