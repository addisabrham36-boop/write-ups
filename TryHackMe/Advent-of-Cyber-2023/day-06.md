# Advent of Cyber 2023 - Day 6: Memories of Christmas Past

{% embed url="https://tryhackme.com/room/adventofcyber2023" %}

## Overview & Investigation Walkthrough

In this task we will be looking at how memory corruption through a buffer overflow vulnerability can be exploited in a web game.

1. If the coins variable had the in-memory value in the image below, how many coins would you have in the game?

   ![Memory](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D6_Memory.png)

   We can see the 4 bytes reprisenting the coin counter are 4f 4f 50 53.

   We can use the 'from base' recipe in Cyberchef to convert this hex value to numbers (base 10). We must select base 16 as our source (hex).

   Since the program uses Little Endian notation for the memory values, we must enter the bytes in reverse order.

   ![Coins](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D6_Coins.png)

   ><details><summary>Click for answer</summary>1397772111</details>

2. What is the value of the final flag?

   At the beginning of the game, we have one ornament and one coin. The computer can be used to gather more coins.

   ![Game Begin](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D6_Game_Begin.png)

   Looking at the memory debug panel, we see the various variables and their contents such as our player name and coin count.

   ![Game Inventory](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D6_Game_Inventory.png)

   To check the buffer overflow vulnerability we should try a name that is longer that the amount of bytes that er reserverd in this game. In this case that would be 13 characters or more.

   We could even choose a name that is 12 characters long and add some characters to get a coin count we can calculate beforehand.

   `My Name Here` is 12 characters long. Using Cyberchef we can calculate how many coins the string `ab` would result in.

   ![Game Extra](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D6_Game_Extra.png)

   So changing our character name to `My Name Hereab` should result in 25185 coins.

   First we must get enough coins and then we can change our name.

   ![Game Name](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D6_Game_Name.png)

   ![Game Inventory 2](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D6_Game_Inventory_2.png)

   Success! Now we can try and buy ourselves a star to get the flag.

   ![Game No Star](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D6_Game_No_Star.png)

   Bummer, looks like it doesn't let us buy a star this way. Unfortunately, it takes away our coins and gives us some other ornament.

   Perhaps we can give our character a name that would also give is a star. It must then be long enough to overflow all the way into the inventory memory.

   Checking the ornament ID list we can see that a star has ID 'D'.

   Lets create a character name that gives us a star and lamas, while leaving the names of our shopkeepers the same.

   ```cmd
   My Name Hereab Van Frosty  Van Holly   1234d44444444444444
   |_____________||__________||__________|    ||____________|
          v            v            v         v      v
      my name       shop 1       shop 2     star    lamas
   ```

   ![Game Better Name](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D6_Game_Better_Name.png)

   ![Game Success](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D6_Game_Success.png)

   Success! Looks like we have an inventory full of lamas and a star.

   Now lets head to the tree and get our flag.

   ![Game Tree](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D6_Game_Tree.png)

   ><details><summary>Click for answer</summary>THM{mchoneybell_is_the_real_star}</details>

We have only explored the surface of buffer overflows in this task. Buffer overflows are the basis of many public exploits and can even be used to gain complete control of a machine. If you want to explore this subject more in-depth, feel free to check the [Buffer Overflows](https://tryhackme.com/room/bof1) room.

Van Jolly still thinks the Ghost of Christmas Past is in the game. She says she has seen it with her own eyes! She thinks the Ghost is hiding in a glitch, whatever that means. What could she have seen?

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| If the coins variable had the in-memory value in the image below, how many coins would you have in the game? | `1397772111` |
| What is the value of the final flag? | `flag{hidden}` |

> **Note:** Flags are redacted as `flag{hidden}` in accordance with responsible disclosure guidelines. The step-by-step commands above provide the verified reproduction path.
