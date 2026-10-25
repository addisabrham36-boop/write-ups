# Wi-Fi Fundamentals and Reconnaissance — TryHackMe

Room: https://tryhackme.com/room/wifundamentals

Wi-Fi reconnaissance is about understanding wireless networks visible to a device and the information their access points advertise. It can help administrators map their own wireless environment, find rogue access points, and understand which security modes are in use.

## Wireless basics

The SSID is the network name, while the BSSID identifies a specific access point radio. Channels determine where the radio operates, and the security configuration determines how clients authenticate and protect traffic.

In a controlled lab, tools such as a wireless scanner can display nearby access points and their advertised details. I should use the interface and commands specified by the room, because the available options depend on the adapter, driver, and operating system.

## Security lessons

A hidden SSID is not a strong security control because the network may still be discoverable from other wireless traffic. Use modern WPA2-AES or WPA3 settings, strong unique credentials, current firmware, and network segmentation. Wireless testing should be limited to equipment and networks I own or am explicitly authorised to assess.

## What I took from this room

Wireless reconnaissance reveals useful configuration information, but detection of a network does not grant permission to attack it. The goal is to understand the environment and improve security while keeping the testing authorised and controlled.