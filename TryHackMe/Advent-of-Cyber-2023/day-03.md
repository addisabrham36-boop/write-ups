# Advent of Cyber 2023 - Day 3: Hydra is Coming to Town

{% embed url="https://tryhackme.com/room/adventofcyber2023" %}

## Overview & Investigation Walkthrough

In this task we will be using Hydra to bruteforce our way into the security system for the IT server room.

When trying the access the login page, make sure to use `http` (not https) and append the port number to the ip address.

So if your ip is `10.10.10.10` and your provided port number is `8000`, you need to visit `http://10.10.10.10:8000`.

1. Using crunch and hydra, find the PIN code to access the control system and unlock the door. What is the flag?

   The first thing we need to do is generate our wordlist using crunch.

   ```cmd
   crunch 3 3 0123456789ABCDEF -o pins.txt
   ```

   ![Pins](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D3_Pins.png)

   Now we use this list in hydra to bruteforce the page. First we need some more info about the login page.

   ![Form](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D3_Form.png)

   We can see the page we need is `/login.php` and the it is a `POST` form. The name of the input field is `pin`. This we can now use to formulate our hydra command.

   ```CMD
   hydra -l '' -P pins.txt 10.10.121.183 http-post-form "/login.php:pin=^PASS^:F=denied" -t 4 -s 8000
   ```

   I also added `-l ''` to indicate there is no username and `-s 8000` to indicate the port to use.

   ![Password](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D3_Password.png)

   Using this password we can get access to the system and unlock the door!

   ![Flag](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D3_Flag.png)

   ><details><summary>Click for answer</summary>THM{pin-code-brute-force}</details>

If you have enjoyed this room please check out the [Password Attacks](https://tryhackme.com/room/passwordattacks) room.

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| Using crunch and hydra, find the PIN code to access the control system and unlock the door. What is the flag? | `flag{hidden}` |

> **Note:** Flags are redacted as `flag{hidden}` in accordance with responsible disclosure guidelines. The step-by-step commands above provide the verified reproduction path.
