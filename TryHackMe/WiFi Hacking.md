# WiFi Hacking: TryHackMe Walkthrough

Room: https://tryhackme.com/room/wifihacking

Wireless security testing examines how a Wi-Fi network authenticates clients and protects traffic. The exact tools and steps depend on the lab hardware, driver support, and the protocol being tested. All active tests must remain within the network and devices explicitly assigned by the room.

## 1. Check the lab requirements first

Wireless exercises may require a compatible adapter, monitor-mode support, and a lab network that is intentionally configured for testing. A normal laptop's built-in adapter may not support every feature. If the room provides an AttackBox or simulated environment, follow its instructions instead of assuming local hardware will work.

Record the lab's target SSID, BSSID, channel, and authorised scope. These values identify the test network. Do not use a real neighbour's network as a substitute.

## 2. Understand what is being tested

Wireless assessments commonly examine:
- Whether the network uses a secure protocol such as WPA2-AES or WPA3.
- Whether the passphrase is strong and unique.
- Whether clients validate the correct access point or authentication server.
- Whether guest devices are isolated from internal systems.
- Whether management interfaces and router firmware are secure.
- Whether monitoring can detect rogue access points or suspicious authentication activity.

A visible SSID does not mean a network is open, and a captured authentication exchange does not automatically reveal the password. The security result depends on the protocol, configuration, and password strength.

## 3. Passive observation versus active testing

Passive observation listens for wireless frames and may reveal network names, access-point identifiers, channels, and client behaviour. Active testing transmits frames or interacts with the network and can disrupt connectivity. Only perform the active actions specifically required by the authorised lab.

When reviewing a capture, identify the frame type and the context. Management frames help devices discover and associate with access points; control frames help coordinate access to the medium; data frames carry higher-layer traffic. Protected data should not be assumed readable simply because it was captured.

## 4. Interpret authentication carefully

Personal Wi-Fi commonly uses a pre-shared passphrase. Enterprise networks often use 802.1X with an authentication server and individual credentials or certificates. Weak passwords, obsolete encryption, misconfigured enterprise authentication, and poor client validation create different risks.

Do not treat a generic command or a tutorial's output as proof that the lab succeeded. Use the exact procedure provided by the room, inspect the actual output, and explain what the evidence establishes.

## 5. Troubleshooting in a lab

- If the adapter is not visible, check whether the lab supports the hardware and whether the correct interface is selected.
- If monitor mode is unavailable, confirm driver and adapter support rather than repeatedly running unrelated commands.
- If the target is not visible, verify the correct lab is active, the channel is correct, and the adapter can observe that band.
- If the task result differs from a guide, check the target details and room version. Wireless lab environments can change.
- If a test disrupts connectivity, stop and restore the environment according to the lab instructions.

## 6. Defences

Use modern encryption, a long unique passphrase, updated router firmware, strong administrative credentials, and guest-network isolation. Enterprise deployments should validate authentication certificates and avoid insecure fallback modes. Monitor for rogue access points and unusual association patterns.

## 7. Working through the room

Follow the room's exact scope and identify what the task asks you to observe or verify. Use the provided SSID and BSSID, then record the observed protocol and security property. If the exercise asks about a weakness, explain the condition that makes it possible and the configuration change that addresses it.

## What I took from this room

Wireless testing requires more than knowing a tool name. I need to understand the protocol, confirm the target, distinguish passive observation from disruptive actions, and tie every conclusion to evidence from the authorised lab.