# Advent of Cyber 2023 - Day 16: Can't CAPTCHA this Machine!

{% embed url="https://tryhackme.com/room/adventofcyber2023" %}

## Overview & Investigation Walkthrough

In this task we are using Machine Learning to create a model that can successfully solve CAPTCHAs for us to bruteforce a login portal.

1. What key process of training a neural network is taken care of by using a CNN?

   The answer to this question can be found in the text.

   ><details><summary>Click for answer</summary>Feature Extraction</details>

2. What is the name of the process used in the CNN to extract the features?

   The answer to this question can be found in the text.

   ><details><summary>Click for answer</summary>Convolution</details>

3. What is the name of the process used to reduce the features down?

   The answer to this question can be found in the text.

   ><details><summary>Click for answer</summary>Pooling</details>

4. What off-the-shelf CNN did we use to train a CAPTCHA-cracking OCR model?

   The answer to this question can be found in the text.

   ><details><summary>Click for answer</summary>Attention OCR</details>

5. What is the password that McGreedy set on the HQ Admin portal?

   On the webpage provided, we can find the portal we need to hack into.

   ![Portal](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D16_Portal.png)

   We can do the steps needed to extract our data and train the model, but since that has already been done for us, we can simply export our trained model.

   But first, we can test it to see what its performance is.

   Lets run the container:

   ```cmd
   docker run -d -v /tmp/data:/tempdit/ aocr/full
   docker ps
   docker exec -it 3030ebad1623
   ```

   ![Connect Docker](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D16_Connect_Docker.png)

   This should now have given us a shell into our container.

   Here we can perform the testing of the model on our test data.

   ```cmd
   cd /ocr/labels/
   aocr test testing.tfrecords
   ```

   ![Testing](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D16_Testing.png)

   It looks like our model is doing well. Only a few incorrect answers are given.

   We can export the model to the tmp folder.

   ```cmd
   cd /ocr/model
   cp -r model /tempdir/
   ```

   Now we can exit the container and close it.

   ```cmd
   exit
   docker kill <Container ID>
   ```

   Next we must run the tensorflow docker container.

   ```cmd
   docker run -t --rm -p 8501:8501 -v /tmp/data/model/exported-model:/models/ -e MODEL_NAME=ocr tensorflow/serving
   ```

   This will run with our model mounted to the models folder.

   Finally we can run our script using:

   ```cmd
   cd ~/Desktop/bruteforcer/
   python3 bruteforce.py 
   ```

   ![Bruteforce](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D16_Bruteforce.png)

   The model only had two incorrect CAPTCHA guesses and we found the password in the end.
   
   ><details><summary>Click for answer</summary>ReallyNotGonnaGuessThis</details>

7. What is the value of the flag that you receive when you successfully authenticate to the HQ Admin portal?

   With the password found, we can log in into the portal.

   ![Sing In](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D16_Sing_In.png)

   Success, we are in!

   We are now given our flag.

   ![Flag](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D16_Flag.png)

   ><details><summary>Click for answer</summary>THM{Captcha.Can't.Hold.Me.Back}</details>

If you enjoyed this room, check out our Red Teaming learning path!

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| What key process of training a neural network is taken care of by using a CNN? | `Feature Extraction` |
| What is the name of the process used in the CNN to extract the features? | `Convolution` |
| What is the name of the process used to reduce the features down? | `Pooling` |
| What off-the-shelf CNN did we use to train a CAPTCHA-cracking OCR model? | `Attention OCR` |
| What is the password that McGreedy set on the HQ Admin portal? | `ReallyNotGonnaGuessThis` |
| What is the value of the flag that you receive when you successfully authenticate to the HQ Admin portal? | `flag{hidden}` |

> **Note:** Flags are redacted as `flag{hidden}` in accordance with responsible disclosure guidelines. The step-by-step commands above provide the verified reproduction path.
