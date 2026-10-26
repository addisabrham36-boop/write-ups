# Advent of Cyber 2022 - Day 19: Hardware Hacking

{% embed url="https://tryhackme.com/room/adventofcyber4" %}

## Background
Hardware security, analyzing raw electrical signals, and comparing serial protocols: **USART**, **SPI**, and **I2C**.

## Walkthrough
Key serial communication concepts:
- **Logic Analyzer:** Hardware device used to capture and decode electrical voltage transitions on signal lines.
- **SPI (Serial Peripheral Interface):** Full-duplex, high-speed, 4-wire bus (MOSI, MISO, SCK, CS). Faster than USART and I2C.
- **I2C (Inter-Integrated Circuit):** 2-wire serial bus (SDA, SCL).
- **USART (Universal Synchronous/Asynchronous Receiver-Transmitter):** Point-to-point, typically 2 wires (TX, RX). Slower than SPI and I2C.

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| Device used to probe electrical signals between two devices? | `Logic Analyser` |
| USART is faster than SPI for communication? (Yea/Nay) | `Nay` |
| USART communication uses fewer wires than SPI? (Yea/Nay) | `Yea` |
| USART is faster than I2C for communication? (Yea/Nay) | `Nay` |
