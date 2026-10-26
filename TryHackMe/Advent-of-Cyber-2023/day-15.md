# Advent of Cyber 2023 - Day 15: Jingle Bell SPAM: Machine Learning Saves the Day!

{% embed url="https://tryhackme.com/room/adventofcyber2023" %}

## Overview & Investigation Walkthrough

In this task we will look at a Machine Learning model that we can train as an email spam filter.

1. What is the key first step in the Machine Learning pipeline?

   The answer to this question can be found in the text.

   ><details><summary>Click for answer</summary>Data Collection</details>

2. Which data preprocessing feature is used to create new features or modify existing ones to improve model performance?

   The answer to this question can be found in the text.

   ><details><summary>Click for answer</summary>Feature Engineering</details>

3. During the data splitting step, 20% of the dataset was split for testing. What is the percentage weightage avg of precision of spam detection?

   After training our data, we must evaluate its performance. We do this by running the code below step 5 in Jupyter.

   ![Precision](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D15_Precision.png)

   Unfortunately, the value we get from this is not the answer they are looking for. This is probably due to the fact that each model can be different in ML. So instead will can use the validation data provided to us in the text.

   ![Precision Correct](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D15_Precision_Correct.png)

   ><details><summary>Click for answer</summary>0.98</details>

5. How many of the test emails are marked as spam?

   Now we can use this model to find spam in our test data set by running the corresponding code in Jupyter.

   ![Test Result](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D15_Test_Result.png)

   We can see that three of the emails have been marked as spam.

   ><details><summary>Click for answer</summary>3</details>

7. One of the emails that is detected as spam contains a secret code. What is the code?

   Lets add a couple line to our notebook that will give us our flag.

   To print the spam emails, we can print the results where the prediction is marked as spam:

   ```python
   for i,x in enumerate(results_df['Prediction']):
    if results_df['Prediction'][i] == 'spam':
        print(results_df['Messages'][i])
        print('-----------------------')
   ```

   ![Flag](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D15_Flag.png)

   ><details><summary>Click for answer</summary>I_HaTe_BesT_FestiVal</details>

If you enjoyed this room, please check out the [Phishing](https://tryhackme.com/module/phishing) module.

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| What is the key first step in the Machine Learning pipeline? | `Data Collection` |
| Which data preprocessing feature is used to create new features or modify existing ones to improve model performance? | `Feature Engineering` |
| During the data splitting step, 20% of the dataset was split for testing. What is the percentage weightage avg of precision of spam detection? | `0.98` |
| How many of the test emails are marked as spam? | `3` |
| One of the emails that is detected as spam contains a secret code. What is the code? | `I_HaTe_BesT_FestiVal` |

> **Note:** Flags are redacted as `flag{hidden}` in accordance with responsible disclosure guidelines. The step-by-step commands above provide the verified reproduction path.
