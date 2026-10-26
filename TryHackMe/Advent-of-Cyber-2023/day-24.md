# Advent of Cyber 2023 - Day 24: You Are on the Naughty List, McGreedy

{% embed url="https://tryhackme.com/room/adventofcyber2023" %}

## Overview & Investigation Walkthrough

In this task we will take a look at how we can analyse an Android image using Autopsy.

1. One of the photos contains a flag. What is it?

   To start, we need to create a new case in Autopsy and import the image. Fortunately, this has already been done for us. So we can open the case  "Tracy McGreedy".

   ![D24 Open Case](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D24_Open_Case.png)

   We can look through the photos on the phone in the file tree we can filter on the photos. One of these photos contains a flag.

   ![D24 Flag](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D24_Flag.png)

   ><details><summary>Click for answer</summary>THM{DIGITAL_FORENSICS}</details>

2. What name does Tracy use to save Detective Frost-eau’s phone number?

   Under contacts we can look for any saved contacts. 

   ![D24 Contact Name](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D24_Contact_Name.png)

   ><details><summary>Click for answer</summary>Detective Carrot-Nose</details>

3. One SMS exchanged with Van Sprinkles contains a password. What is it?

   Under messages there are various messages sent and received. One of these is a message sent by Tracy to Van Sprinkles.

   ![D24 Password](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D24_Password.png)

   ><details><summary>Click for answer</summary>chee7AQu</details>

If you have enjoyed this room please check out the [Autopsy](https://tryhackme.com/room/btautopsye0) room.



### Day 24 Jolly Judgement Day

The final step is to get a conviction in court. To do so we must provide the correct evidence and answer some question related to them.

1. What is the final flag? 

   To get the flag, we must provide the correct evidence and answer the correct question in each step of the trail.

   **Question 1**

   _Judge: Mr. McGreedy, the opposition claims you masterminded a revenge plot against the company. What do you say to that?_

   The information we got from the chatbox indicates he is working on some secret plan.

   ![Judgement 1](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_Judgement_1.png)

   It is called 

   ![Judgement 1 Question 1](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_Judgement_1_Question_1.png)

   **Question 2**

   _Judge: Mr. McGreedy, the opposition claims you have been using your old hacker handle in your activities, which is how they were able to identify your accounts. Is this correct?_

   We could see evidence of this handle on the start screen on the MS-DOS box as well as the forum post about exploits.

   ![Judgement 2](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_Judgement_2.png)

   ![Judgement 2 Question 1](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_Judgement_2_Question_1.png)
   
   ![Judgement 2 Question 2](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_Judgement_2_Question_2.png)

   **Question 3**

   _Judge: The court is informed of an extensive investigation that started after the USB incident, and has uncovered a trail leading to a command-and-control server central to this cyber activity. Mr. McGreedy, are you aware of or connected to this server? Your input could be vital in clarifying this case._

   The information we found off of the usb stick can confirm this.

   ![Judgement 3](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_Judgement_3.png)

   It also led us to a C2 server.
   
   ![Judgement 3 Question 1](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_Judgement_3_Question_1.png)

   **Question 4**

   _Judge: Mr. McGreedy, you're claiming you're being framed, but the opposition emphasizes your technical skills and describes you as being capable of leading such a cyber operation. They claim to have proof for you orchestrating attacks on AntarctiCrafts and Best Festival Company._

   The malware sample was downloaded from a domain with a familiar name. From the server password takeover we could see the connection came from his machine.

   ![Judgement 4](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_Judgement_4.png)
   
   ![Judgement 4 Question 1](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_Judgement_4_Question_1.png)
   
   ![Judgement 4 Question 2](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_Judgement_4_Question_2.png)

   **Question 5**

   _Judge: The court is informed of an extensive investigation that started after the USB incident and has uncovered a trail leading to a command-and-control server central to this cyber activity. Mr. McGreedy, are you aware of or connected to this server? Your input could be vital in clarifying this case._

   The credentials used for the C2 server are connected to his name.

   ![Judgement 5](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_Judgement_5.png)
   
   ![Judgement 5 Question 1](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_Judgement_5_Question_1.png)

   **Question 6**

   _Judge: The evidence so far, though compelling, is circumstantial. It suggests but doesn't conclusively link Mr. McGreedy to the allegations. Does the opposition have more solid evidence that directly ties Mr. McGreedy to these crimes?_

   We have text messages which link him to the crimes.

   ![Judgement 6](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_Judgement_6.png)
   
   ![Judgement 6 Question 1](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_Judgement_6_Question_1.png)

   We did it! We successfully presented our evidence and answered the question in order to get a verdict.
   
   ![Judgement Flag](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_Judgement_Flag.png)

   ><details><summary>Click for answer</summary>THM{YouMeddlingKids}</details>

##

 Congratulations on finishing Advent of Cyber 2023!

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| One of the photos contains a flag. What is it? | `flag{hidden}` |
| What name does Tracy use to save Detective Frost-eau’s phone number? | `Detective Carrot-Nose` |
| One SMS exchanged with Van Sprinkles contains a password. What is it? | `chee7AQu` |
| What is the final flag? | `flag{hidden}` |

> **Note:** Flags are redacted as `flag{hidden}` in accordance with responsible disclosure guidelines. The step-by-step commands above provide the verified reproduction path.
