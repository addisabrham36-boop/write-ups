# Wi-Fi Fundamentals and Reconnaissance

Official room: https://tryhackme.com/room/wififundamentalsandrecon

## Room Overview & Information

**Category:** Wireless security / reconnaissance  
**Difficulty:** Beginner to intermediate  
**Focus:** 802.11 basics, SSIDs and BSSIDs, channels, encryption, client associations, monitor mode, and airodump-ng.

Wireless networks extend an organisation's attack surface beyond the walls of its building. Radio signals can be received outside the physical office, and clients may automatically attempt to reconnect to previously used networks. Reconnaissance helps an authorised assessor understand which networks are present and how they are configured.

Use only the room's provided wireless lab or equipment you own and are authorised to test. Do not capture traffic from other people's networks.

## Phase 1: Wireless terminology

- **SSID:** the network name presented to users.
- **BSSID:** usually the MAC address of a particular access point radio.
- **Channel:** the portion of spectrum used by the network.
- **Client/station:** a device associated with an access point.
- **Beacon:** a management frame used by an access point to advertise network information.
- **Authentication and encryption:** controls that determine who can join and how wireless traffic is protected.

An SSID can be hidden from normal display, but that does not make the network invisible. Other management frames and client behaviour may reveal it.

## Phase 2: Understand wireless security

Open networks provide no Wi-Fi link-layer encryption. WEP is obsolete and insecure. WPA2-Personal commonly uses a pre-shared passphrase, while WPA3-Personal uses a more modern authentication exchange. Enterprise networks commonly use 802.1X/EAP with an authentication server.

A strong passphrase helps protect WPA2-Personal, but it must be unique and not reused across services. Enterprise deployments need proper certificate validation and identity configuration to avoid credential theft through rogue access points.

## Phase 3: Prepare the lab adapter

The room provides a controlled environment. Follow its instructions for selecting the wireless interface. On Linux, interface names vary, so inspect the actual system:

```bash
ip link
iw dev
```

- `ip link` lists network interfaces.
- `iw dev` lists wireless devices and their interface names.

Monitor mode allows a compatible wireless adapter to receive management and data frames that ordinary managed mode may not expose. Not all adapters and drivers support monitor mode or packet injection. Do not assume an interface name from another system.

## Phase 4: Use airodump-ng for reconnaissance

In the lab, use the interface and command provided by the room. A typical reconnaissance pattern is:

```bash
sudo airodump-ng INTERFACE
```

Replace `INTERFACE` with the wireless interface identified by the lab. Airodump-ng lists nearby access points and may show BSSID, channel, encryption, authentication, power, and SSID. Associated clients may appear in a separate section.

The output is a snapshot. Signal strength varies with location and antenna, and a network may not appear if the adapter is on the wrong channel or does not support the required mode.

To focus on a lab access point, use the BSSID and channel supplied by the task with the room's instructed options. Avoid broad collection beyond the assigned environment.

## Phase 5: Identify hidden SSIDs and client behaviour

A hidden SSID suppresses the name in some beacon frames, but clients and access points may reveal the name during other management exchanges. In the lab, compare the AP and client information shown by the capture tool and follow the room's instructions to identify the requested network.

Do not infer a hidden SSID from a random nearby network. Confirm that the BSSID and client association correspond to the lab target.

## Phase 6: Answering the practical questions

For any question asking for an SSID, BSSID, channel, encryption type, or client, inspect the current airodump-ng output:
1. Confirm the correct interface is being monitored.
2. Locate the target access point by the details supplied by the room.
3. Read the requested column rather than a neighbouring one.
4. If the SSID is hidden, follow the lab's client/management-frame exercise.
5. Record the value exactly as displayed.

The target values depend on the lab's wireless environment. This write-up explains the workflow rather than inventing BSSIDs, client MACs, or hidden network names.

## Phase 7: Defensive recommendations

- Use WPA3 or WPA2 with a strong, unique passphrase where appropriate.
- Prefer enterprise authentication with correctly validated certificates for managed organisations.
- Disable WEP and other obsolete security modes.
- Keep access-point firmware updated.
- Segment guest Wi-Fi from internal resources.
- Monitor for rogue access points and unexpected clients.
- Avoid relying on hidden SSIDs or MAC allowlists as security controls.
- Review client auto-join settings and remove unused saved networks.

## Key takeaways

Wireless reconnaissance is about understanding what the radio environment reveals. Airodump-ng helps map access points and clients, but each observation must be tied to the authorised lab target. Hidden SSIDs and MAC filtering are not substitutes for strong authentication and encryption.

## Phase 1: Reconnaissance methodology

Use the wireless lab equipment and network assigned to the room. Begin by identifying the adapter and its capabilities before attempting monitor mode:

    ip link
    iw dev
    iw list
    rfkill list

The output establishes the actual interface name, supported bands, and whether the radio is blocked. Do not assume the interface is named wlan0; modern Linux systems commonly use predictable interface names.

## Phase 2: Understand the visible wireless fields

When the room's capture tool lists access points, record SSID, BSSID, channel, frequency/band, security suite, and signal strength. SSID is a network name; BSSID normally identifies a particular AP radio. One physical access point may advertise multiple SSIDs or use multiple radios.

A hidden SSID is not a security boundary. Client probe or association behaviour can reveal it. MAC filtering is also weak as an identity control because addresses can be observed and spoofed.

## Phase 3: Monitor mode and evidence

Follow the room's instructions to enable monitor mode on the lab adapter. Confirm the interface state before starting capture. Use the specified channel and lab access point; changing channels can cause the capture to miss relevant frames.

In airodump-ng output, access-point rows and client/station rows describe different entities. The BSSID column on an AP row identifies the AP radio; on a client row it may identify the AP the client is associated with. Read the row label and column context before answering a question.

Capture only the traffic required by the exercise. Record the capture filename, tool version, and the exact row or packet that supports each answer.

## Phase 4: Interpret security posture

WEP is obsolete. WPA2-Personal depends on a pre-shared key and good configuration; WPA3-Personal uses SAE to improve resistance to offline guessing. Enterprise authentication can improve identity management but must be configured with correct certificate validation. An SSID name or a visual icon is not proof of the security protocol.

## Phase 5: Questions and remediation

Use the live lab output for requested SSIDs, BSSIDs, channels, bands, protocols, or client counts. Do not copy a value from another run because nearby APs and channel assignments can differ.

Defensive measures include WPA3 or properly configured WPA2-AES, unique credentials, current firmware, disablement of unnecessary WPS, guest isolation, secure AP administration, rogue-AP monitoring, and proper certificate validation for enterprise networks.
