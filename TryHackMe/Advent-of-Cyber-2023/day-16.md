# Advent of Cyber 2023 - Day 16: Machine Learning

{% embed url="https://tryhackme.com/room/adventofcyber2023" %}

## Background
Automating CAPTCHA breaking using Convolutional Neural Networks (CNNs) and Optical Character Recognition (OCR).

## Walkthrough
1. Convolutional Neural Networks automate **Feature Extraction**.
2. **Convolution** extracts mathematical feature maps; **Pooling** reduces spatial dimensionality.
3. The model architecture demonstrated is **Attention OCR**.
4. Run the trained solver against the HQ Admin login portal. The automated script bypasses the CAPTCHA protection, enters password `ReallyNotGonnaGuessThis`, and logs in to reveal: `THM{Captcha.Can't.Hold.Me.Back}`.

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| Key process taken care of by using a CNN? | `Feature Extraction` |
| Name of process used in CNN to extract features? | `Convolution` |
| Name of process used to reduce features down? | `Pooling` |
| Off-the-shelf CNN used for CAPTCHA OCR? | `Attention OCR` |
| Password set on HQ Admin portal? | `ReallyNotGonnaGuessThis` |
| Flag received after authenticating? | `THM{Captcha.Can't.Hold.Me.Back}` |
