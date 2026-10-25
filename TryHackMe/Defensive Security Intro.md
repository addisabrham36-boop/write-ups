# Defensive Security Intro: TryHackMe Walkthrough

Official room: https://tryhackme.com/room/dsicyber1013n3

## Room Overview & Information

**Category:** Defensive security / SOC  
**Difficulty:** Beginner  
**Scenario:** Investigate suspicious activity against FakeBank, identify the source and the attacker's latest URL discovery attempt, then block the source using the simulated firewall.

This room demonstrates the defensive workflow: detect, investigate, contain, and then improve the controls. The dashboard is a simulated lab, so use the values displayed in your active instance. The source IP and latest URL are evidence to read from the dashboard, not values to guess from a write-up.

## Task 1: Become the Defender

The Security Operations Centre (SOC) is the central team responsible for monitoring and responding to security events. A SOC combines people, processes, and tools such as alert dashboards, SIEM platforms, endpoint telemetry, and incident-response procedures.

**Question: “What does the acronym ‘SOC’ stand for?”**

Answer: **Security Operations Centre** (also written *Security Operations Center*).

A SOC does not simply collect alerts. Analysts triage signals, establish whether activity is malicious, determine scope and impact, coordinate containment, and document the result.

## Task 2: Phase 1: An Attack Begins

Open the monitoring dashboard using the room's **View Site** button. The scenario reports hundreds of login attempts against FakeBank. A baseline describes expected activity; unusual deviations can create alerts, but an alert alone does not prove an attack.

**Question: “Review the ‘Suspicious Login’ alert. What username is being used?”**

1. Open the dashboard from the room.
2. Locate the **Suspicious Login** alert.
3. Open or expand its details.
4. Find the username field in the event data.
5. Copy the value exactly as shown.

Do not substitute a common username or use the source IP as the answer. The username belongs to the alert evidence in the live simulator.

## Task 3: Phase 2: An Attack Is Investigated

The analyst must determine where the activity originates. Inspect the alert and related events in the dashboard, then identify the source address associated with the suspicious login attempts.

When reviewing an event, distinguish:
- **Source IP:** where the observed connection appears to originate.
- **Destination:** the system being accessed.
- **Username:** the account name involved in authentication.
- **Timestamp:** when the event occurred.
- **Action or result:** whether the attempt succeeded, failed, or triggered another event.

If the room asks for the suspicious source IP, copy it from the relevant alert. NAT, proxies, and VPNs can complicate attribution in real environments, so an IP is a useful indicator, not automatic proof of a person's identity.

## Task 4: Phase 3: Identify the Attack

The room asks you to inspect the **URL Discovery Attempts** list and examine the latest entry.

**Question: “Copy the latest URL that the attacker has tried to find and paste it below.”**

1. Open the **URL Discovery Attempts** list in the dashboard.
2. Sort or inspect the entries by their timestamps.
3. Select the latest entry, not merely the first URL in the list.
4. Copy the complete URL path shown by the simulator.
5. Submit it exactly as displayed.

Rapid attempts to access many paths are consistent with content discovery or directory enumeration. The attacker may be searching for hidden pages, administrative routes, backups, or other exposed resources. The event list establishes what the simulator observed; it does not prove that the attacker successfully accessed the path.

## Task 5: Phase 4: Stop the Attack

Containment is the immediate step to limit ongoing harm. The lab has already completed two security actions and asks you to add a firewall rule for the suspicious source IP shown in the practical.

The current lab instructions specify the IP `32.122.195.63`. Confirm that the simulator still displays this value before applying the rule.

1. Open the firewall-rule control.
2. Enter the suspicious source IP.
3. Choose **BLOCK** from the action dropdown.
4. Press **Apply**.
5. Wait for the success message.
6. Copy the flag shown by the active room and submit it there.

Blocking the address prevents the simulated source from continuing to connect. In a real environment, an IP block can also affect shared NAT gateways or legitimate users, so responders should assess the impact and monitor whether the activity moves to another source.

## Phase 6: Verify containment and continue the investigation

After blocking, verify that the simulator confirms the rule was applied. A success message is evidence that the configuration change was accepted; in a production incident, the analyst would also check firewall logs and subsequent events to confirm that traffic was actually blocked.

Containment is not the same as eradication. After the immediate attack is stopped, the organisation should investigate how the attacker reached the system, patch the underlying weakness, review authentication events, and monitor for persistence or alternate source addresses.

## Defensive analysis

The observed rapid URL discovery attempts suggest the attacker is mapping the application's exposed paths. Useful follow-up actions in a real SOC include:
- Preserve the relevant alert and request logs.
- Check whether any discovered paths returned sensitive data.
- Review successful logins and account changes near the same time.
- Apply rate limits or web application protections where appropriate.
- Fix access-control weaknesses rather than relying only on blocking one IP.
- Record the decision, time, rule, expected effect, and rollback procedure.

## Key takeaways

This room walks through the SOC cycle from alert to containment. Identify the account and source from the alert, inspect the latest URL discovery event, block the confirmed source in the simulated firewall, and verify the change. Use the active dashboard for dynamic values and do not invent a flag or event result.