# AI Models and Data — TryHackMe

Room: https://tryhackme.com/room/aimodels

AI systems learn patterns from data and use a model to produce predictions or outputs. The quality, provenance, and handling of that data can affect the system's behaviour and security.

## Data and training

Training data is used to fit model parameters. Validation data helps tune the system, while test data is used to estimate performance on examples that were not used during training. If these datasets overlap in the wrong way, reported performance can be misleading.

Data can contain bias, errors, sensitive information, or maliciously crafted examples. A model may also behave poorly when it encounters inputs that differ from its training distribution.

## Model behaviour

A model's output is influenced by its architecture, training data, fine-tuning, and runtime instructions. A plausible response is not necessarily a correct response. Systems should be evaluated on realistic tasks and edge cases, including misuse scenarios.

## Security and privacy

Training or inference data may contain confidential information. Access should be limited, retention should be considered, and data should be handled according to its sensitivity. Teams also need to think about poisoning, leakage, and unexpected model behaviour.

## What I took from this room

AI security starts with understanding the data and model lifecycle. Data quality, evaluation, privacy, and access controls all affect the final system. It is not enough to test whether a model gives a good answer on a few friendly examples.