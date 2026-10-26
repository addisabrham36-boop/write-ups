# Advent of Cyber 2024 - Day 5: SOC-mas XX-what-ee?

{% embed url="https://tryhackme.com/room/adventofcyber2024" %}

## Overview & Investigation Walkthrough

1. What is the flag discovered after navigating through the wishes?

   Lets begin by setting up the proxy in our browser for Burpsuite to intercept the requests. If we navigate the platform, we can see requests are being intercepted.

   ![Proxy](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_5_Proxy.png)

   To modify the request, we should first make a request to add a product to our wishlist. This is then visible in the history list.

   ![Wishlist](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_5_Wishlist.png)

   Now we can send this request to Repeater so we can start exploiting the vulnerability.

   ![Repeater](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_5_Repeater.png)

   To use the vulnerability, we must add the following to the request.

   ```xml
   <!--?xml version="1.0" ?-->
   <!DOCTYPE foo [<!ENTITY payload SYSTEM "/var/www/html/wishes/wish_1.txt"> ]>
   ```

   Remember to also add the payload within the product element, `&payload;`.

   ![Payloads](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_5_Payloads.png)

   ![Request 1](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_5_Request_1.png)

   As you can see we get the contents of wishlist number one.
   
   To find the wishlist containing the flag, we can continue modifying these requests until we fin the correct one or we can automate this using Intruder.

   Lets send the request to Intruder and add a payload marker.

   ![Intruder](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_5_Intruder.png)

   In the payloads section, we should add a number list with a start to finish of 1 and 21, with an increment of 1.

   Now we can start the attack and it will make all the requests for us. Looking through each one untill we find our flag.

   ![Flag 1](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_5_Flag_1.png)

   ><details><summary>Click for answer</summary>THM{Brut3f0rc1n6_mY_w4y}</details>

2. What is the flag seen on the possible proof of sabotage?

   To fing our second flag we can navigate to the changelog file.

   http://10.10.148.95/CHANGELOG

   ![Flag 2](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_5_Flag_2.png)

   ><details><summary>Click for answer</summary>THM{m4y0r_m4lw4r3_b4ckd00rs}</details>

3. If you want to learn more about the XXE injection attack, check out theXXEroom!

4. Following McSkidy's advice, Software recently hardened the server. It used to have many unneeded open ports, but not anymore. Not that this matters in any way.

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| What is the flag discovered after navigating through the wishes? | `flag{hidden}` |
| What is the flag seen on the possible proof of sabotage? | `flag{hidden}` |

> **Note:** Flags are redacted as `flag{hidden}` in accordance with responsible disclosure guidelines. The step-by-step commands above provide the verified reproduction path.
