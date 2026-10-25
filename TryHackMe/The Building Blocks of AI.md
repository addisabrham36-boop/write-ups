# The Building Blocks of AI: TryHackMe Walkthrough

Room: https://tryhackme.com/room/thebuildingblocksofai

This room introduces the terms used to describe artificial intelligence and machine learning. The most useful starting point is to distinguish the task a system performs from the process used to build it. AI is the broad field; machine learning is one approach within it, and deep learning is a family of machine-learning methods based on neural networks.

## 1. Understand the lifecycle

A basic machine-learning workflow looks like this:

1. Collect and prepare data.
2. Choose a model and training approach.
3. Train the model using examples.
4. Evaluate it on data that was not used to fit it.
5. Deploy the model for inference.
6. Monitor its behaviour and update it when needed.

Training adjusts model parameters. Inference uses the trained model to produce an output for new input. A model that performs well on its training examples may still fail on new data, so evaluation is essential.

## 2. Match the task to the model output

- **Classification:** assigns an input to a category, such as spam or not spam.
- **Regression:** predicts a numeric value, such as a demand estimate.
- **Clustering:** groups similar examples without relying on predefined labels in the same way as supervised classification.
- **Generative modelling:** creates new content such as text, images, audio, or code.
- **Reinforcement learning:** learns a policy through interaction and feedback such as rewards.

These categories are not mutually exclusive in every modern system. They are useful descriptions of the main objective.

## 3. Understand data and labels

Supervised learning uses labelled examples. Unsupervised learning looks for patterns or structure without the same kind of target labels. Training data teaches the model; validation data helps tune choices; and test data estimates performance after development decisions have been made.

Poor labels, missing groups, biased collection, or maliciously altered examples can damage a model. If test data leaks into training, reported performance may be overly optimistic. The quality and origin of data matter as much as the model architecture.

## 4. Neural networks and deep learning

A neural network consists of connected computational units arranged in layers. Each layer transforms its input, and training adjusts weights to reduce a loss function. Deep learning uses networks with multiple layers to learn complex patterns.

This does not mean the model reasons exactly like a human brain. The biological analogy is limited. A model learns statistical patterns from data and can still produce incorrect or nonsensical outputs.

## 5. Overfitting, underfitting, and generalisation

- **Overfitting:** the model learns training-specific details and performs poorly on new examples.
- **Underfitting:** the model is too limited or insufficiently trained to capture useful patterns.
- **Generalisation:** the ability to perform on new examples from the intended environment.
- **Distribution shift:** production inputs differ from the training or evaluation data.

Accuracy is not always enough. A classifier used for fraud detection may need precision, recall, false-positive rates, and an assessment of the cost of different errors. The right metric depends on the problem.

## 6. Limitations and security

AI output is not automatically correct or unbiased. Models may reveal sensitive information, respond badly to unusual inputs, or behave differently after the deployment environment changes. Generative models can also produce plausible statements that have no factual basis.

Security depends on the entire application: data provenance, access control, model supply chain, input handling, output validation, and monitoring. A model's fluent answer should be checked before it is used for important decisions.

## 7. Working through the room

When a task describes a model or workflow, identify its input, output, and training method. If it describes categories such as spam detection, numeric forecasting, or generating text, match the task to the corresponding model objective. If it asks about training, validation, or test data, focus on the role each dataset plays.

Use the terminology and examples provided by the active room. Do not assume the same wording appears in every version of a learning platform.

## What I took from this room

AI is a collection of approaches, not one single technology. Understanding data, training, inference, evaluation, and model limitations makes it easier to recognise both the useful capabilities and the security risks of AI systems.

## Expanded AI concepts and security workflow

### 1. Distinguish the terminology

Artificial intelligence is the broad field of systems performing tasks associated with intelligent behaviour. Machine learning learns patterns from data. Deep learning uses multi-layer neural networks. Generative AI produces content such as text, images, code, or audio. These categories overlap but are not interchangeable.

### 2. Training, inference, and evaluation

Training adjusts model parameters using data and an optimization procedure. Inference uses a trained model to produce output for an input. Evaluation measures behaviour against a dataset or criteria. A model can perform well on its evaluation set and still fail on unusual, biased, or adversarial inputs.

### 3. Data and model behaviour

Training data quality, labels, preprocessing, architecture, model parameters, prompts, and runtime settings influence outcomes. A surprising answer can result from a normal model limitation, missing context, malicious input, unsafe retrieval, or a compromised component. Identify the mechanism rather than assuming one cause.

### 4. Common risks

Risks include biased outputs, hallucinated facts, privacy leakage, prompt injection, unsafe tool calls, poisoned data, compromised model supply chains, and service abuse. The surrounding application determines whether a model error becomes a security incident.

### 5. Security controls

Use data provenance, access controls, secure deployment, model versioning, human review for high-impact decisions, output validation, and monitoring. Do not let generated code or tool arguments bypass normal authorization or validation.

### 6. Room answers

Use definitions and examples from the active room. When a question asks about training versus inference or AI versus machine learning, identify the process or scope described instead of relying on loose everyday language. Redact flags as flag{hidden}.
