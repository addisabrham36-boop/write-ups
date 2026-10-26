# Advent of Cyber 2024 - Day 19: I merely noticed that you’re improperly stored, my dear secret!

{% embed url="https://tryhackme.com/room/adventofcyber2024" %}

## Overview & Investigation Walkthrough

1. What is the OTP flag?

   First we should change the javascript hook for the OTP challenge. This is located in the set_otp file.

   Here we should add a line to log the arguments that are being sent for the function.

   ```js
   log('Parameter:' + args[0].toInt32());
   ```

   ![Js Otp](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_19_Js_Otp.png)

   Now we can start the game again using frida and intercept the argument.

   ```cmd
   frida-trace ./TryUnlockMe -i 'libaocgame.so!*'
   ```

   ![Console Otp](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_19_Console_Otp.png)

   We can see when interacting with the penguin, the argument is logged in the console. Using this we can complete the first challenge and get the first flag.

   ![Flag Otp](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_19_Flag_Otp.png)

   ><details><summary>Click for answer</summary>THM{one_though_password}</details>

2. What is the billionaire item flag?

   Continuing with the second challenge, we should again add some lines to validate_purchase. This passes three integer arguments.

   ```js
   log('Parameter 0: ' + args[0].toInt32());
   log('Parameter 1: ' + args[1].toInt32());
   log('Parameter 2: ' + args[2].toInt32());
   ```

   ![Js Purchase](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_19_Js_Purchase.png)

   Again we run the game with frida and can intercept the argument from this function.

   ![Console Purchase](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_19_Console_Purchase.png)

   We can see three arguments indeed. The first one is related to the chosen item, the second is related to the item cost, and the third argument is related to the players money.

   We should add a pointer to alter the price to be zero.

   ```js
   args[1] = ptr(0)
   ```

   ![Js Purchase Alter](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_19_Js_Purchase_Alter.png)

   Running the game again, we can see the selected item has a price of 0! Now we can get the second flag.

   ![Flag Purchase](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_19_Flag_Purchase.png)

   ><details><summary>Click for answer</summary>THM{credit_card_undeclined}</details>

3. What is the biometric flag?

   For our final challenge we need to log the argument. But this is a bit trickier since it is not an integer. Logging the arguments like last time doesn't give anything meaningfull. So we try again by logging the return value.

   ```js
   log('The return value is: ' + retval)
   ```

   ![Js Bio](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_19_Js_Bio.png)

   ![Console Bio](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_19_Console_Bio.png)

   Now we can see a value being returned of `0x0`. As mentioned, this could be a bolean and we should alter it to be 1. This can be achieved by replacing the retval with our pointer.

   ```js
   retval.replace(ptr(1))
   ```

   ![Js Bio Alter](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_19_Js_Bio_Alter.png)

   When trying the final challenge we see we pass the challenge and get our final flag.

   ![Flag Bio](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_19_Flag_Bio.png)

   ><details><summary>Click for answer</summary>THM{dont_smash_your_keyboard}</details>

4. If you liked today's task, you can practice your skills with "Memories of Christmas Past" fromAdvent of Cyber 2023.

5. The second penguin gave pretty solid advice. Maybe you should listen to him more.

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| What is the OTP flag? | `flag{hidden}` |
| What is the billionaire item flag? | `flag{hidden}` |
| What is the biometric flag? | `flag{hidden}` |

> **Note:** Flags are redacted as `flag{hidden}` in accordance with responsible disclosure guidelines. The step-by-step commands above provide the verified reproduction path.
