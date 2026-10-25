# RA: Oh My WebServer — TryHackMe

Room: https://tryhackme.com/room/ohmywebserver

This machine challenge is about investigating an exposed web service and following the evidence provided by the target. I should confirm the exact room page and the active target before assuming that a particular exploit path applies.

## Initial scan

With the lab target assigned, I can start with:

```bash
nmap -sV -sC -oN webserver-scan.txt TARGET_IP
```

The output helps identify ports and service versions. I should inspect the actual results and check whether the exposed software is affected by a known issue. A version banner alone is not sufficient proof, because banners can be incomplete or misleading.

## Validate carefully

If the room directs me to a known vulnerability, I should understand what the proof of concept does before running it. Testing should remain limited to the room's target, and I should avoid destructive actions. After access is obtained, I need to inspect the current account and permissions before investigating any further escalation path.

## What I took from this room

Web server exploitation depends on the service version, configuration, and evidence from enumeration. The reliable method is to identify the service, validate the suspected weakness, and document the result rather than assume a generic exploit will work on every server.