# Wi-Fi Fundamentals and Reconnaissance: TryHackMe Walkthrough

Room: https://tryhackme.com/room/wififundamentals

This note combines the fundamentals needed to interpret wireless reconnaissance. Wi-Fi uses radio communication to connect clients to an access point, but security depends on more than the SSID or signal strength. Authentication, encryption, client validation, and network segmentation all matter.

## 1. Identify a wireless network

An SSID is the network name presented to users. A BSSID identifies a specific access point radio, often represented by a MAC address. One SSID can be provided by multiple access points, so the SSID alone does not uniquely identify the radio.

A wireless scan may reveal SSIDs, BSSIDs, channels, supported security modes, and signal strength. These are observations, not proof that the network is vulnerable. Use only the lab network and values provided by TryHackMe.

## 2. Understand the bands

Wi-Fi commonly uses 2.4 GHz and 5 GHz, with 6 GHz supported by newer equipment in regions where it is permitted. The 2.4 GHz band often has more range but is more crowded. Higher-frequency bands may offer more capacity but can have less range through walls.

Channel use and interference affect performance. A strong signal does not guarantee a reliable connection, and a visible network name does not tell you whether the authentication configuration is secure.

## 3. Authentication and encryption

WEP is obsolete and insecure. WPA2 with AES-CCMP and WPA3 are modern options, depending on device compatibility and configuration. Enterprise Wi-Fi may use 802.1X and a central authentication service, while personal Wi-Fi often uses a shared passphrase.

A long unique passphrase helps protect personal networks. Enterprise clients should validate authentication-server certificates; otherwise, a malicious access point may be able to impersonate a trusted network.

## 4. Passive observation

Passive wireless reconnaissance listens for frames without deliberately disrupting clients. Management frames help devices discover and associate with access points; control frames help coordinate wireless communication; data frames carry higher-layer traffic.

A capture may show network metadata even when payloads are encrypted. Do not assume that capturing a frame reveals the application data. Interpret the frame type and the security mode before drawing conclusions.

## 5. Active testing and scope

Active testing transmits frames or interacts with the network and may disconnect clients or change state. Only perform active tests specifically authorised by the room. Never use a real nearby network as a substitute for the training target.

If the lab provides a wireless interface, follow its instructions to select the correct device and channel. Adapter and driver support vary, so an unavailable feature may be a hardware limitation rather than a mistake in the command.

## 6. Defensive checks

- Use WPA2-AES or WPA3 and disable obsolete modes.
- Set a unique, strong passphrase or use enterprise authentication.
- Validate certificates for enterprise Wi-Fi.
- Update access-point firmware and change administrative defaults.
- Separate guest clients from trusted internal devices.
- Monitor for rogue access points and unusual association attempts.
- Avoid exposing router administration to untrusted networks.

## 7. Working through the room

Read the task and identify whether it asks for an SSID, BSSID, channel, frequency, security protocol, or observation from a capture. Use the exact values displayed in the active lab. If the task asks you to infer a weakness, explain which configuration or behaviour supports that conclusion.

## What I took from this room

Wireless reconnaissance provides useful clues, but each clue needs context. The network name, radio identifier, frame type, authentication method, and encryption configuration answer different questions and should not be treated as interchangeable.