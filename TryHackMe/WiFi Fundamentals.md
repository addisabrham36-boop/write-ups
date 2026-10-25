# Wi-Fi Fundamentals: TryHackMe Walkthrough

Room: https://tryhackme.com/room/wififundamentals

Wi-Fi lets devices connect to a local network using radio waves. The same network concepts still apply: devices need addressing, a gateway, DNS, and access controls. The wireless link adds its own concerns, including radio coverage, authentication, encryption, and the possibility of nearby devices observing wireless traffic.

## 1. Understand the wireless network

An access point advertises a wireless network identified by an SSID. A client scans for networks, authenticates, associates with an access point, and then typically receives network configuration through DHCP. The SSID is a network name, not a secret, and hiding it does not provide meaningful security.

A BSSID identifies a particular access point radio interface, commonly represented by a MAC address. A single SSID can be provided by multiple access points, each with its own BSSID. Channels and bands affect how nearby networks share the radio spectrum.

## 2. Frequency bands and channels

Wi-Fi commonly operates in the 2.4 GHz and 5 GHz bands, with newer equipment also supporting 6 GHz where regulations and hardware permit. The 2.4 GHz band generally travels farther through obstacles but has fewer non-overlapping channels and often more interference. Higher-frequency bands can offer more capacity but may have shorter practical range.

Channel selection should consider local interference, device support, and regulatory requirements. A channel number by itself does not guarantee better performance.

## 3. Authentication and encryption

Older protocols such as WEP are insecure and should not be used. WPA2 with AES-CCMP remains common, while WPA3 provides newer protections when supported and correctly configured. Enterprise Wi-Fi commonly uses 802.1X and an authentication server rather than one shared household password.

A strong passphrase matters for personal Wi-Fi security. Use a unique password, update router firmware, disable obsolete protocols, and avoid leaving the administration interface exposed to untrusted networks.

## 4. Inspect the local connection

On Linux, commands such as nmcli device status, nmcli connection show, and ip addr can help inspect network interfaces and assigned addresses. The exact output depends on the distribution and network manager.

If Wi-Fi is connected but internet access fails, check the interface, IP address, default route, DNS resolution, and service reachability in that order. A wireless association does not prove that DHCP or DNS is working.

## 5. Wireless security risks

Common risks include weak passphrases, obsolete encryption, rogue access points, evil-twin networks, insecure router administration, and poor separation between guest and internal devices. A rogue access point may imitate a legitimate network name to trick users into connecting, but the SSID alone is not proof of legitimacy.

Defences include WPA2-AES or WPA3, certificate validation for enterprise authentication, strong router credentials, firmware updates, guest-network isolation, and monitoring for unexpected access points. Users should be cautious with unfamiliar networks and avoid bypassing certificate warnings.

## 6. Work through the room

Read each task to identify whether it refers to an SSID, BSSID, frequency band, channel, authentication method, or encryption protocol. If the room presents a network scan, use the values shown in that environment. Do not substitute the details of a nearby real network.

Any active wireless testing should remain inside the authorised lab. Do not capture traffic, disconnect clients, or test passwords against networks you do not own or have explicit permission to assess.

## What I took from this room

Wi-Fi is not just a password and a signal bar. Security depends on the authentication method, encryption, router configuration, radio environment, and the way wireless clients are isolated from other systems.