# Advent of Cyber 2022 - Day 10: Memory Manipulation

{% embed url="https://tryhackme.com/room/adventofcyber4" %}

## Background
Game hacking, memory scanning, and modifying runtime process variables in WebAssembly / browser games to bypass gameplay barriers.

## Walkthrough
1. Launch the web-based Yeti mini-game.
2. Open the in-game developer/memory console (`Cetus` or browser memory inspector).
3. Search for variables corresponding to player health, inventory, and coins.
4. Modify memory values directly:
   - Freeze player health at `999`.
   - Update player score to surpass the Guard's threshold.
5. Talking to the Guard with modified score unlocks the Guard's flag: `THM{5_star_fl4gzzz}`.
6. Defeating the Yeti boss using infinite health reveals the Yeti's flag: `THM{yetiyetiyetiflagflagflag}`.

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| What is the Guard's flag? | `THM{5_star_fl4gzzz}` |
| What is the Yeti's flag? | `THM{yetiyetiyetiflagflagflag}` |
