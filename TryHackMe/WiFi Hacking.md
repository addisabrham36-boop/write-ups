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

## Room task evidence checklist

| Task focus | What to confirm before answering |
|---|---|
| Wireless adapter | The actual interface name and supported monitor-mode capabilities |
| Access-point discovery | SSID, BSSID, channel, and security mode from the lab |
| Authentication test | The exact authorised access point and task-supplied method |
| Captured traffic | Whether the required frames are present and belong to the target BSSID |
| Password or flag question | The value reported by the active lab, not a guessed or reused value |

A captured exchange is not the Wi-Fi passphrase. A failure to capture traffic may reflect the interface, channel, timing, or driver rather than a secure access point.

## Final remediation checklist

Use WPA3 or properly configured WPA2-AES, strong unique credentials, current firmware, network segmentation, and secure administration. Disable unnecessary WPS, monitor for rogue access points, and never rely on a hidden SSID or MAC allowlist as the primary access-control mechanism.
