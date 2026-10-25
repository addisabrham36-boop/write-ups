# Wi-Fi Fundamentals: TryHackMe Walkthrough

Official room: https://tryhackme.com/room/wififundamentals

## Room Overview & Information

**Category:** Wireless networking and security  
**Difficulty:** Beginner  
**Focus:** Wi-Fi architecture, SSIDs/BSSIDs, radio channels, authentication, encryption, and common wireless risks.

Wi-Fi provides a wireless link between clients and an access point. The access point connects the wireless network to a wired LAN or other network. Because radio signals travel beyond physical walls, wireless security depends on configuration and monitoring as well as physical access controls.

## Phase 1: Identify the wireless network

The **SSID** is the human-readable network name. The **BSSID** identifies a particular access point radio, commonly using its MAC address. A single organisation may use the same SSID across multiple access points, each with its own BSSID.

A channel identifies the radio-frequency channel used by the access point. Channel availability and overlap depend on the band and regulatory region. A client and access point need compatible radio settings to communicate.

## Phase 2: Authentication and encryption

- **Open Wi-Fi:** no Wi-Fi password-based authentication and generally no link-layer encryption.
- **WEP:** legacy encryption that is considered broken and should not be used.
- **WPA2-Personal:** commonly authenticates with a shared pre-shared key.
- **WPA3-Personal:** uses a stronger modern authentication design.
- **WPA/WPA2-Enterprise:** commonly uses 802.1X and EAP with an authentication server.

Encryption protects the wireless link, but it does not guarantee that every destination or application is safe. HTTPS and application-level protections remain important.

## Phase 3: Understand the handshake at a high level

In a WPA2-Personal network, the client and access point perform a four-way handshake that helps confirm possession of the shared secret and derives session keys. A captured handshake does not immediately reveal the passphrase. Offline guessing is possible if an attacker captures the required exchange and the passphrase is weak.

This is why a long, unique passphrase matters. A passphrase reused elsewhere can also be compromised through an unrelated breach.

## Phase 4: Recognise common wireless risks

- Weak or default Wi-Fi passwords.
- Obsolete encryption modes.
- Rogue access points that impersonate a trusted network.
- Misconfigured enterprise certificate validation.
- Poor guest-network isolation.
- Exposed management interfaces.
- Unpatched access-point firmware.
- Clients automatically reconnecting to untrusted networks.

A hidden SSID does not provide meaningful access control. Likewise, MAC allowlists can be bypassed because MAC addresses are not strong proof of identity.

## Phase 5: Inspect a wireless configuration

On a Linux system you administer, use:

```bash
iw dev
ip link
nmcli device wifi list
```

- `iw dev` lists wireless interfaces.
- `ip link` shows interface state.
- `nmcli device wifi list` displays networks visible to NetworkManager.

The output depends on the adapter, driver, permissions, location, and whether wireless is enabled. Use the interface and target values provided by the TryHackMe lab rather than copying an example from another computer.

## Phase 6: Work through the room

Read each task and identify whether it asks about a network name, access-point identifier, security mode, authentication method, or defensive control. Use the interactive content provided by the room. If the question requests a live value, copy it from the lab output.

For a practical question, verify the selected network by BSSID or other lab-specific evidence. Similar SSIDs may exist nearby, so do not assume the first matching name is the intended target.

## Phase 7: Secure a wireless deployment

Use WPA3 or properly configured WPA2, disable obsolete protocols, set unique credentials, update firmware, and separate guest devices from sensitive systems. For enterprise Wi-Fi, validate server certificates and enforce identity-based access. Monitor for rogue access points and remove unused accounts or saved network profiles.

## Key takeaways

Wi-Fi is a radio-based network link with its own discovery, authentication, and encryption mechanisms. Security depends on modern protocols, strong credentials, careful client configuration, and network segmentation. SSID hiding and MAC filtering should never be treated as the primary security boundary.

## Expanded wireless practical guide

### 1. Identify the network elements

The SSID is the network name presented to users. The BSSID identifies a particular access point radio, usually by its MAC address. A client scans supported channels, discovers beacons or probe responses, associates with an access point, and completes the configured authentication and key-establishment process.

### 2. Frequency bands and channels

Wi-Fi can operate in the 2.4 GHz, 5 GHz, and supported 6 GHz bands. Available channels and power rules depend on region and hardware. The 2.4 GHz band often has more interference and fewer non-overlapping channels; higher bands can provide more capacity but differ in propagation and compatibility.

Do not infer the frequency from the channel number alone because channel numbering differs by band. Use the lab output or access-point configuration.

### 3. Check a Linux wireless device

```bash
ip link
iw dev
iw list
```

These commands list interfaces, wireless devices, and reported device capabilities. Results depend on the driver and adapter. If the lab provides a preconfigured attack machine, do not change your everyday Wi-Fi configuration unnecessarily.

### 4. Authentication and encryption

WEP is obsolete. WPA2-Personal commonly relies on a pre-shared passphrase, while WPA3-Personal uses SAE. Enterprise networks commonly use 802.1X/EAP and an authentication server. Correct encryption helps protect wireless traffic, but weak credentials, vulnerable firmware, and poor network segmentation can still expose systems.

### 5. Troubleshoot by layer

If a client cannot connect, check whether it can see the SSID, whether the radio supports the band, whether authentication succeeds, whether DHCP assigns an address, and whether DNS resolves names. Each stage indicates a different class of issue. A wrong password and a missing DHCP response should not be diagnosed the same way.

### 6. Use the active room as evidence

For each task, identify the exact item being requested: SSID, BSSID, frequency, channel, protocol, authentication method, or security property. Read the relevant field from the lab instead of guessing from a network name or a generic example. Do not copy values from another environment.

### 7. Secure configuration

Use current firmware, modern WPA settings, a unique passphrase, protected administrator access, guest-network isolation, and monitoring for unexpected access points. Disable WPS when not required. Segment wireless clients according to trust, and restrict access from guest networks to internal resources.
