# Wi-Fi Hacking: TryHackMe Walkthrough

Official room: https://tryhackme.com/room/wifihacking

## Room Overview & Information

**Category:** Wireless security testing  
**Difficulty:** Beginner  
**Focus:** Wireless reconnaissance, adapter modes, access-point identification, authentication weaknesses, and defensive lessons.

Wireless testing must be performed only on the access point and client environment supplied by the room or on equipment you own and have permission to test. Capturing or interfering with a neighbour's wireless traffic is not an acceptable substitute for a lab.

## Phase 1: Confirm the test environment

Follow the room's instructions to start the wireless lab and identify the interface and target access point. Wireless tools often require a compatible adapter and elevated permissions. If the lab runs in a browser-based VM, use the interface provided by that VM rather than assuming your physical laptop's Wi-Fi adapter is passed through.

Check available interfaces:

```bash
ip link
iw dev
```

If the expected adapter is missing, verify the lab setup and driver support before attempting any wireless operation.

## Phase 2: Understand the target

Record the SSID, BSSID, channel, and security mode from the room's interface. The SSID is a network name; the BSSID identifies a particular access-point radio. Similar network names may belong to different devices, so confirm the BSSID or other task-specific identifier.

The security mode tells you what kind of authentication and encryption is configured. WEP is obsolete; WPA2-Personal depends on the strength of the shared passphrase; enterprise Wi-Fi uses a different identity and authentication model.

## Phase 3: Wireless reconnaissance

In an authorised lab with the required adapter support, a wireless discovery tool can list access points and clients. Use the exact interface and options specified by the active task. Record the results before changing anything.

A typical tool output may include:
- BSSID and SSID.
- Channel and signal strength.
- Encryption and authentication information.
- Client devices associated with the AP.
- Management-frame observations.

These fields are evidence about the environment, not proof that a network is vulnerable. An access point's presence does not authorise testing it.

## Phase 4: Follow the room's practical

Use the lab's supplied steps for the challenge. Identify which condition the task is testing, such as weak authentication, unsafe configuration, or the ability to observe a particular management-frame exchange. Test one hypothesis at a time and verify the result in the simulator.

Do not use deauthentication or packet-injection techniques against networks outside the lab. Those techniques can interrupt legitimate users and may be illegal without explicit authorisation.

If the room asks for a specific network property or flag, obtain it from the current task interface. Wireless values and challenge state can vary between instances, so this write-up does not substitute guessed BSSIDs, passphrases, or flags.

## Phase 5: Troubleshooting

- **No access point appears:** confirm the adapter is present, the correct interface is selected, and the lab is active.
- **Monitor mode is unavailable:** check adapter and driver support; not every virtual adapter supports it.
- **The target appears on a different channel:** confirm the task's BSSID and channel and follow the lab's instructions.
- **No client appears:** there may be no active client, or the capture may not be observing the relevant channel.
- **A captured exchange does not prove a password:** verify what the evidence demonstrates; a handshake is not the plaintext passphrase.

## Phase 6: Defensive recommendations

Use modern Wi-Fi security, strong unique credentials, enterprise certificate validation where applicable, firmware updates, network segmentation, and monitoring for rogue access points. Disable unused WPS functionality if it is not required, and review saved client profiles to reduce automatic connection to untrusted networks.

## Key takeaways

Wireless testing combines radio reconnaissance with careful interpretation of authentication and configuration. The first job is to identify the exact authorised target and record evidence. The final conclusion should match what the lab actually demonstrates, not what a tool might theoretically be able to do.

## Expanded wireless assessment workflow

### 1. Confirm the lab boundary

Wireless testing can affect nearby devices, so use the isolated access point and adapter supplied by the room. Record the target SSID, BSSID, channel, security mode, and permitted actions before collecting frames. Never capture or attack an unrelated network.

### 2. Identify the adapter

On the provided Linux environment, inspect available interfaces:

```bash
ip link
iw dev
rfkill list
```

`ip link` lists network interfaces, `iw dev` lists wireless devices and their current configuration, and `rfkill list` shows whether software or hardware has disabled a radio. Adapter and driver capabilities vary; not every device supports monitor mode or packet injection.

### 3. Understand the protocol

WEP is obsolete and insecure. WPA2-Personal commonly derives session keys from a pre-shared passphrase. WPA3-Personal uses SAE to make offline password guessing more difficult. Enterprise networks typically use 802.1X/EAP rather than a shared household passphrase.

A captured handshake is not the Wi-Fi password. Depending on the protocol and capture, it may provide material for testing candidate passphrases offline. The security weakness is often a weak or reused password, not the mere existence of a capture.

### 4. Capture and validate only the lab traffic

Follow the room's adapter, channel, and capture instructions exactly. Record the capture filename and inspect it with the tool provided by the lab. Before interpreting a result, confirm that the capture contains frames from the intended BSSID and the exchange required by the task.

If no relevant frames appear, check the selected interface, channel, adapter support, signal, and capture timing. A failed capture does not prove the access point is secure; it may simply mean the required exchange was not recorded.

### 5. Interpret findings carefully

Distinguish network discovery from authentication and authentication from successful address assignment. A client may discover an SSID but fail to authenticate; it may authenticate but fail DHCP. Report the observed stage rather than describing every connection problem as a password issue.

For a password-checking exercise, use only the room's supplied capture and wordlist. Validate a candidate through the room's intended method, and never publish credentials for a real network.

### 6. Room questions and evidence

For questions about an SSID, BSSID, channel, encryption standard, or captured event, read the value from the live lab's output. Save the relevant command, tool version, capture file hash, and a short output excerpt. Use `flag{hidden}` in public notes instead of exposing a flag.

### 7. Remediation

Use WPA3 when compatible, or properly configured WPA2-AES where needed. Choose a long, unique passphrase, update access-point firmware, disable WPS if unnecessary, isolate guest devices, protect administrative access, and monitor rogue access points. Hidden SSIDs and MAC filtering are not substitutes for strong authentication.
