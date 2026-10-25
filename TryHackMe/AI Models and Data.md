# AI Models and Data: TryHackMe Walkthrough

Room: https://tryhackme.com/room/aimodels

This room is about the relationship between data, models, and the behaviour of an AI system. I keep the distinction clear because many AI security problems are described as “model problems” when the actual weakness starts with the data pipeline, access controls, or the way the model is deployed.

## 1. Understand the data lifecycle

A typical machine-learning project has several datasets and stages:

- **Training data** is used to fit the model's parameters.
- **Validation data** helps compare configurations and tune the model.
- **Test data** estimates how well the final model performs on examples it was not trained or tuned on.
- **Inference data** is supplied when the deployed model is being used.

These sets serve different purposes. If examples from the test set leak into training, the evaluation can look better than real-world performance. If training data is collected without checking its origin, the model may learn biased, inaccurate, or deliberately manipulated patterns.

While working through the room, I read each question carefully and identify which stage it describes. “Used to train the model” points to training data; “used to tune or select a model” points to validation data; and “final unbiased evaluation” points to test data.

## 2. Separate model types and tasks

A model learns a mapping from input to output. A classification model assigns an input to a category, a regression model predicts a numeric value, and a generative model creates new content such as text or images. A large language model is a generative model trained to process and produce language.

The model's answer is not automatically a fact. Outputs depend on the training process, prompt, context, decoding settings, and deployment logic. For questions that compare model behaviour, focus on the task and the kind of output rather than assuming every AI system works like a chatbot.

## 3. Data quality and data leakage

When assessing a dataset, I look for:

1. **Relevance:** Does it represent the task and the environment where the model will be used?
2. **Quality:** Are labels, records, and values accurate and consistent?
3. **Representativeness:** Are important groups or situations missing?
4. **Provenance:** Where did the data come from, and was its use authorised?
5. **Sensitivity:** Does it contain personal information, credentials, or confidential business data?
6. **Separation:** Were training, validation, and test records kept appropriately separate?

Data leakage happens when information that would not legitimately be available at prediction time influences training or evaluation. It can make a model appear excellent in testing while failing in production.

## 4. Security threats to data and models

- **Data poisoning:** an attacker alters training or reference data to influence future outputs.
- **Sensitive information disclosure:** private data is exposed through training, retrieval, logs, or inference.
- **Model extraction:** repeated queries or access to model files are used to reproduce a model.
- **Model inversion or inference:** outputs are used to infer sensitive details about training data or individuals.
- **Distribution shift:** real-world inputs differ from the data on which the model was trained.
- **Supply-chain compromise:** a model, dataset, dependency, or pre-trained artifact is replaced or tampered with.

These threats need different controls. Dataset provenance and review help reduce poisoning risk; least-privilege access and data minimisation reduce exposure; and model registries, checksums, versioning, and trusted artifact sources help protect the supply chain.

## 5. A practical way to work through the room

1. Open the official room and read the task text before answering.
2. Identify whether the question is about data, model behaviour, evaluation, privacy, or security.
3. Match the wording to the lifecycle stage or threat category.
4. If the task provides an interactive example, inspect the data or scenario it presents rather than guessing from the title.
5. Submit the answer in the room's requested format and read the explanation after it is accepted.
6. Record the concept and the reason it is correct in these notes. Do not copy private lab data into a public repository.

I do not invent a flag or a successful result when I have not observed it in the live room. If the room's question or UI changes, use the explanation above to reason from the current evidence.

## 6. Defences and operational checks

A responsible AI data process should document dataset owners and sources, restrict access, scan for secrets, remove data that is not needed, preserve versions, and evaluate models on representative held-out data. Production systems also need monitoring because performance and input distributions can change over time.

For model artifacts, record the source, version, hash, dependencies, and approval status. Do not load unknown model files or packages on a normal workstation just because a tutorial says to try them.

## What I took from this room

AI security begins before a prompt is sent. Data provenance, quality, evaluation, privacy, and model lifecycle controls influence the safety and reliability of the finished system. A good benchmark score is only useful when the evaluation is sound and the deployed system is protected.