# Advent of Cyber 2023 - Day 6: Memory Corruption

{% embed url="https://tryhackme.com/room/adventofcyber2023" %}

## Background
Introduction to memory layouts, stack buffers, endianness (Little Endian vs. Big Endian), and variable overwriting.

## Walkthrough
1. In the target game challenge, player coin inventory is represented by a 32-bit integer in memory.
2. The memory editor reveals the raw hexadecimal bytes: `4F 4B 54 53`.
3. Converting from Little Endian to Big Endian representation:
   - Little Endian: `4F 4B 54 53`
   - Hex value: `0x53544B4F`
   - Decimal conversion: `1397772111`
4. Exploiting an unchecked string input buffer overwrites adjacent variables, granting sufficient inventory to complete the quest.

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| Coins value after converting in-memory bytes to decimal? | `1397772111` |
| Game completion flag | `THM{MEM_CORRUPTION_GAME}` |
