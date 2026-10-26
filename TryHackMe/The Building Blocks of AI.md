# The Building Blocks of AI

Official room: https://tryhackme.com/room/thebuildingblocksofai

## Room Overview & Information

**Category:** Artificial intelligence and machine learning fundamentals  
**Difficulty:** Beginner  
**Purpose:** Explain the main AI terms, how models learn from data, how neural networks and large language models work, and why those distinctions matter for security.

This is a conceptual room with knowledge checks, not an exploitation VM. Nmap, shell access, and privilege escalation are not part of the learning objective. The right method is to connect each question to the definition and example in the room rather than force an attack workflow onto a theory lesson.

## Phase 1: Introduction and Terminology

### Task 1: Introduction

Artificial intelligence (AI) is the broad field of building systems that perform tasks associated with intelligence, such as recognising patterns, making predictions, understanding language, or generating content. AI is not one algorithm or one product.

Machine learning (ML) is a subfield of AI in which a system learns patterns from data rather than relying only on rules written by a programmer. Deep learning is a family of machine-learning methods based on neural networks with multiple layers. A large language model (LLM) is a model trained to process and generate language, usually using a transformer architecture.

### Key distinction

| Term | Meaning | Example |
|---|---|---|
| Artificial intelligence | Broad field of intelligent-behaviour systems | A planning or decision-support system |
| Machine learning | Learning patterns from data | A classifier trained on spam examples |
| Deep learning | ML using multi-layer neural networks | Image recognition using a neural network |
| Generative AI | Models that generate new content | Text, image, audio, or code generation |
| LLM | Large-scale model trained for language tasks | Summarising a document or answering a question |

These categories overlap. Generative AI can use deep learning, and an LLM is one kind of generative model. Avoid treating the terms as synonyms.

## Phase 2: Data and Machine Learning

### Task 2: How machine learning works

A typical ML lifecycle has six stages:

1. **Collect data:** gather examples that relate to the task.
2. **Prepare data:** remove errors, format fields, handle missing values, and define labels where needed.
3. **Train a model:** use data to adjust model parameters.
4. **Evaluate the model:** measure performance on held-out examples.
5. **Deploy for inference:** use the trained model on new inputs.
6. **Monitor and improve:** detect changes in data, errors, and performance over time.

Training and inference are different. Training changes the learned model parameters. Inference uses the trained model to produce an output for a new input. A system can run inference without retraining the model for each request.

### Four common learning approaches

**Supervised learning** uses labelled examples, such as emails marked “spam” or “not spam”. The model learns to predict the label for new examples.

**Unsupervised learning** finds structure in data without relying on the same kind of target labels. Clustering similar network events is one example.

**Semi-supervised learning** combines a smaller set of labelled examples with a larger set of unlabelled data.

**Reinforcement learning** learns behaviour through actions and feedback, often represented as rewards or penalties. It is used when a system must learn a policy through interaction.

### Classification and regression

Classification predicts a category, such as whether a network event is suspicious. Regression predicts a numeric quantity, such as expected demand. Clustering groups similar examples. These are different problem types and should not be mixed up with the learning strategy used to train the model.

### Training, validation, and test data

Training data is used to fit the model. Validation data helps compare choices during development. Test data estimates performance after model-selection decisions have been made. If test data leaks into training, the reported result can be overly optimistic.

Data quality matters. Missing data, poor labels, collection bias, duplicates, or manipulated examples can lead to poor or unfair outcomes. Data poisoning is a security risk in which an attacker deliberately contaminates training data to influence the resulting model.

## Phase 3: Neural Networks and Deep Learning

### Task 3: Neural networks

A neural network is composed of computational units arranged in layers. The input layer receives data, hidden layers transform the representation, and the output layer produces the model's result. Connections have weights that training adjusts to reduce an error or loss function.

The term “neural” is inspired by the brain, but a neural network does not work exactly like a biological brain. It is a mathematical model that learns statistical patterns from examples.

A deeper network can learn complex intermediate representations. Deep learning usually refers to neural networks with multiple layers, often more than three layers under the simplified definition used in introductory materials. The exact boundary is a teaching convention rather than a universal scientific threshold.

### Common failure modes

- **Overfitting:** the model memorises details of training examples and performs poorly on new data.
- **Underfitting:** the model is too simple or inadequately trained to learn the important pattern.
- **Poor generalisation:** performance falls on inputs that differ from the training data.
- **Distribution shift or drift:** real-world data changes over time, making prior performance less reliable.
- **Bias:** patterns in the data or the model can produce systematically worse outcomes for some groups.

Accuracy alone may be misleading. Fraud detection, for example, may need precision, recall, false-positive rates, and a review of the cost of different mistakes.

## Phase 4: Large Language Models

### Task 4: LLMs and transformers

A large language model is trained on text and related data to model language patterns. Modern LLMs commonly use the transformer architecture, which uses attention mechanisms to process relationships between tokens in the context.

Text is broken into tokens. A model generates output by predicting a likely next token based on the preceding context, repeatedly building the response. It does not guarantee that a fluent statement is factually true.

LLM development can include several stages:
- **Pre-training:** learn broad language patterns from a large dataset.
- **Fine-tuning:** further train a model for a task or preferred behaviour.
- **Human-feedback alignment:** use feedback or preference data to steer responses toward desired behaviour. RLHF, or reinforcement learning from human feedback, is one alignment technique described in introductory LLM explanations.
- **Inference:** accept a prompt and produce a completion or response.

The model's response also depends on surrounding components such as the system prompt, retrieval documents, tool definitions, decoding settings, and output processing. A model issue and an application design flaw are not always the same thing.

## Phase 5: Room Questions and Answer Method

The room's interactive questions may use wording specific to its current version. Use the task content above to identify the expected concept.

| Question focus | Answer concept | Reason |
|---|---|---|
| Broad field concerned with intelligent behaviour | Artificial intelligence | It is the umbrella field |
| Learning patterns from data | Machine learning | The system learns from examples rather than only fixed rules |
| Multiple layers of connected computational units | Neural network / deep learning | Layers transform learned representations |
| Learning from labelled examples | Supervised learning | Examples include target labels |
| Finding patterns without target labels | Unsupervised learning | Structure is inferred from data |
| Learning through rewards or feedback | Reinforcement learning | The system learns a policy through interaction |
| Fitting parameters using examples | Training | Training changes model parameters |
| Producing output from a trained model | Inference | The model is being used, not necessarily retrained |
| LLM architecture commonly used today | Transformer | Attention helps model token relationships |
| Generating text token by token | Next-token prediction | The model iteratively predicts output tokens |

These are knowledge-check concepts, not a claim that every line is the exact text of the live question. Copy the official question wording and any required answer format from the room UI when submitting. There is no stable machine-specific flag to invent for this conceptual guide.

## Phase 6: Security Lessons

AI security risks follow from the same lifecycle:
- **Data poisoning** tampers with training examples.
- **Model extraction** uses repeated queries to approximate a model's behaviour.
- **Privacy leakage** occurs when sensitive information is exposed by the model or surrounding application.
- **Prompt injection** attempts to influence the model using attacker-controlled instructions.
- **Model drift** describes performance degradation as the environment changes.
- **Supply-chain compromise** can affect datasets, libraries, model artifacts, and updates.

Secure AI systems require trusted data provenance, access control, safe artifact loading, careful evaluation, privacy protections, and monitoring after deployment. Do not allow a model's text output alone to authorize sensitive actions. The API or tool that owns a resource must independently enforce permissions.

## Key takeaways

AI is the broad field; machine learning learns patterns from data; deep learning uses multi-layer neural networks; and LLMs commonly use transformers to generate language token by token. The distinction between training and inference helps explain how a model is built and how it is used. Security depends not just on the model, but also on data quality, provenance, application permissions, tool access, and ongoing monitoring.
