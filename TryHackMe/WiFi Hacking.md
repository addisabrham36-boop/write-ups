# WiFi Hacking — TryHackMe

Room: https://tryhackme.com/room/wifihacking

Wireless security testing examines how Wi-Fi networks advertise themselves, authenticate clients, and protect traffic. These techniques can disrupt real networks or expose private information, so I should practise only with the lab's provided environment or equipment I own and am authorised to test.

## Understand the environment

An access point broadcasts a network identity and provides wireless access. The security protocol, authentication method, and passphrase strength all affect how well the network is protected. Older WEP security is broken, and obsolete configurations should be replaced.

Wireless tools can inspect the local radio environment, but a detected network is not an invitation to connect to or attack it. I should identify the exact interface and lab target before following the room's commands.

## Defensive lessons

Use WPA2-AES or WPA3 where supported, choose a long unique passphrase, update firmware, disable obsolete protocols, and separate guest devices from trusted internal systems. Enterprise networks should use individual authentication where possible instead of one shared secret for everyone.

## What I took from this room

Wi-Fi security depends on configuration and authentication, not simply whether a network has a password. I should follow the room's authorised steps and document the actual observations rather than apply wireless attack commands to nearby networks.