# Advent of Cyber 2024 - Day 4: I’m all atomic inside!

{% embed url="https://tryhackme.com/room/adventofcyber2024" %}

## Overview & Investigation Walkthrough

1. What was the flag found in the .txt file that is found in the same directory as the PhishingAttachment.xslm artefact?

   Let's first clear the operational log for sysmon at `Applications and Services => Microsoft => Windows => Sysmon => Operational`.
   
   ![Clear](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_4_Clear.png)

   Now we can run the phishing test using:

   ```powershell
   Invoke-AtomicTest -AtomicTechnique T1566.001 -TestNumbers 1
   ```

   ![Phishing](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_4_Phishing.png)

   Refreshing the event viewer, we can see new events created by the test. One of these is related to the creation of the xlsm file. 
   
   ![Target](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_4_Target.png)

   We can navigate to this path to find the .txt file.

   ![Flag 1](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_4_Flag_1.png)

   ><details><summary>Click for answer</summary>THM{GlitchTestingForSpearphishing}</details>

2. What ATT&CK technique ID would be our point of interest?

   On the MITRE Attack Framework webpage, we can search for any techniques involving "Command and Scripting Interpreter".

   ![Mitre](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_4_Mitre.png)

   ><details><summary>Click for answer</summary>T1059</details>

3. What ATT&CK subtechnique ID focuses on the Windows Command Shell?

   On this technique page, we can find a subtechnique covering Windows Command Shell.

   ><details><summary>Click for answer</summary>T1059.003</details>

4. What is the name of the Atomic Test to be simulated?

   We can find out which tests are performed within this technique using:

   ```powershell
   Invoke-AtomicTest -AtomicTechnique T1059.003 -ShowDetailsBrief
   ```

   ![Brief](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_4_Brief.png)

   Since we are looking to conduct a test related to ransomware, our test of interest will be number 4.

   ><details><summary>Click for answer</summary>Simulate BlackByte Ransomware Print Bombing</details>

5. What is the name of the file used in the test?

   We can get more details on this test by using the following command:

   ```powershell
   Invoke-AtomicTest -AtomicTechnique T1059.003 -TestNumbers 4 -ShowDetails
   ```

   ![Details](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_4_Details.png)

   Looking at the commands being executed we can find which file is used in this test.

   ><details><summary>Click for answer</summary>Wareville_Ransomware.txt</details>

6. What is the flag found from this Atomic Test?

   We can either read the file we found earlier using `type C:\Tools\AtomicRedTeam\atomics\T1059.003\src\Wareville_Ransomware.txt` or we can run the test to see the results.

   ![Test Output](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_4_Test_Output.png)

   We can save the resulting PDF and open it to find our flag.

   ![Flag 2](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_4_Flag_2.png)

   ><details><summary>Click for answer</summary>THM{R2xpdGNoIGlzIG5vdCB0aGUgZW5lbXk=}</details>

7. Learn more about theAtomic Red Teamvia the linkedroom.

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| What was the flag found in the .txt file that is found in the same directory as the PhishingAttachment.xslm artefact? | `flag{hidden}` |
| What ATT&CK technique ID would be our point of interest? | `T1059` |
| What ATT&CK subtechnique ID focuses on the Windows Command Shell? | `T1059.003` |
| What is the name of the Atomic Test to be simulated? | `Simulate BlackByte Ransomware Print Bombing` |
| What is the name of the file used in the test? | `Wareville_Ransomware.txt` |
| What is the flag found from this Atomic Test? | `flag{hidden}` |

> **Note:** Flags are redacted as `flag{hidden}` in accordance with responsible disclosure guidelines. The step-by-step commands above provide the verified reproduction path.
