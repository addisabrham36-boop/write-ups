# Advent of Cyber 2023 - Day 15: Machine Learning

{% embed url="https://tryhackme.com/room/adventofcyber2023" %}

## Background
Building a Natural Language Processing (NLP) spam detection pipeline using Python, Scikit-learn, and TF-IDF feature extraction.

## Walkthrough
1. Machine learning pipeline steps:
   - Data collection $\rightarrow$ Data preprocessing & Feature Engineering $\rightarrow$ Model training $\rightarrow$ Evaluation.
2. In the provided notebook, evaluating the Naive Bayes spam classifier:
   - Weighted average precision on test set: `0.98`.
   - Total test emails classified as spam: `3`.
3. Inspecting the text of the third flagged spam email discloses a secret message: `I_Hate_Best_FestiVal`.

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| Key first step in the ML pipeline? | `data collection` |
| Feature used to create/modify features to improve performance? | `feature engineering` |
| Precision weightage average of spam detection? | `0.98` |
| How many test emails are marked as spam? | `3` |
| Secret code inside the spam email? | `I_Hate_Best_FestiVal` |
