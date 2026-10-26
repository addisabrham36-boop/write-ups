# Advent of Cyber 2023 - Day 14: The Little Machine That Wanted to Learn

{% embed url="https://tryhackme.com/room/adventofcyber2023" %}

## Overview & Investigation Walkthrough

In this task we will be looking at how we can train a simply neural network to make predictions of faulty toys.

1. What is the other term given for Artificial Intelligence or the subset of AI meant to teach computers how humans think or nature works?

   The answer to this question can be found in the text.

   ><details><summary>Click for answer</summary>Machine Learning</details>

2. What ML structure aims to mimic the process of natural selection and evolution?

   The answer to this question can be found in the text.

   ><details><summary>Click for answer</summary>Genetic Algorithm</details>

3. What is the name of the learning style that makes use of labelled data to train an ML structure?

   The answer to this question can be found in the text.

   ><details><summary>Click for answer</summary>Supervised Learning</details>

4. What is the name of the layer between the Input and Output layers of a Neural Network?

   The answer to this question can be found in the text.

   ><details><summary>Click for answer</summary>Hidden Layer</details>

5. What is the name of the process used to provide feedback to the Neural Network on how close its prediction was?

   The answer to this question can be found in the text.

   ><details><summary>Click for answer</summary>Back-Propagation</details>

6. What is the value of the flag you received after achieving more than 90% accuracy on your submitted predictions?

   Lets edit the script on the machine using what we just learned. First we add the code to split the data.

   ```cmd
   train_X, validate_x, train_y, validate_y = train_test_split(X, y, test_size=0.2)
   ```

   ![Split Code](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D14_Split_Code.png)

   Then we add the code to normalize our data.

   ```cmd
   scaler = StandardScaler()
   scaler.fit(train_X)
   
   train_X = scaler.transform(Train_X)
   validate_x = scaler.transform(validate_x)
   test_X = scaler.transform(test_X)
   ```
   
   ![Normalize Code](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D14_Normalize_Code.png)

   Then we add the validation code:

   ```cmd
   clf = MLPClassifier(solver='lbfgs', alpha=1e-5,hidden_layer_sizes=(15,2), max_iter=10000)
   clf.fit(train_X, train_y)
   
   y_predicted = clf.predict(validate_X)
   ```

   ![Validate Code](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D14_Validate_Code.png)

   And the prediction code:

   ```cmd
   y_test_predictions = clf.predict(test_X)
   ```
   
   ![Prediction Code](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D14_Prediction_Code.png)

   We can now run this script to train our model and predict the results of our test data using:

   ```cmd
   python3 detector.py
   ```

   ![Output](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D14_Output.png)

   It looks like our validation came back with an accuracy of 91.42%. This should be enough for the task. Lets upload the output to http://websiteforpredictions.thm:8000/.
   
   ![Upload](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D14_Upload.png)

   Looks like we trained our model successfully and received our flag!
   
   ![Flag](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D14_Flag.png)
   
   ><details><summary>Click for answer</summary>THM{Neural.Networks.are.Neat!}</details>

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| What is the other term given for Artificial Intelligence or the subset of AI meant to teach computers how humans think or nature works? | `Machine Learning` |
| What ML structure aims to mimic the process of natural selection and evolution? | `Genetic Algorithm` |
| What is the name of the learning style that makes use of labelled data to train an ML structure? | `Supervised Learning` |
| What is the name of the layer between the Input and Output layers of a Neural Network? | `Hidden Layer` |
| What is the name of the process used to provide feedback to the Neural Network on how close its prediction was? | `Back-Propagation` |
| What is the value of the flag you received after achieving more than 90% accuracy on your submitted predictions? | `flag{hidden}` |

> **Note:** Flags are redacted as `flag{hidden}` in accordance with responsible disclosure guidelines. The step-by-step commands above provide the verified reproduction path.
