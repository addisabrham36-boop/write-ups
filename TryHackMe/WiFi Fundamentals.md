# WiFi Fundamentals — TryHackMe

Room: https://tryhackme.com/room/wifihacking101

Wi-Fi lets devices communicate over radio rather than a wired connection. The convenience is obvious, but radio signals can be received beyond the room or building where the access point sits. Security therefore depends on authentication, encryption, good configuration, and sensible network design.

## Basic terms

An access point provides wireless network access. The SSID is the network name shown to users. A BSSID identifies a particular wireless access point radio, commonly represented by a MAC address. Channels describe portions of the radio spectrum used for communication.

## Encryption and authentication

Older WEP security is broken and should not be used. WPA2 with AES-CCMP is much stronger when configured properly, and WPA3 improves protections in supported environments. A weak shared passphrase can still be guessed, so encryption alone does not rescue a terrible password.

Enterprise Wi-Fi commonly uses 802.1X authentication with a backend authentication service. This allows individual credentials or certificates rather than one shared password for everyone.

## Defensive checks

I should use a unique, strong Wi-Fi passphrase, update access-point firmware, disable obsolete security modes, and separate guest devices from internal systems. I should also avoid connecting to lookalike networks simply because their SSID resembles a familiar one.

## What I took from this room

Wireless security depends on the protocol, authentication method, configuration, and strength of credentials. In a lab, wireless testing must be limited to the equipment and networks explicitly provided for practice. I should never capture or interfere with networks just because they are within radio range.