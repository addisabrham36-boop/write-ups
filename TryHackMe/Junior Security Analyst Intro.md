# Junior Security Analyst Intro

{% embed url="https://tryhackme.com/room/jrsecanalystintrouxo" %}

An introduction to the role and responsibilities of a Tier 1 SOC Analyst / Triage Specialist, walking through alert evaluation, incident escalation, and firewall block rules.

---

## 1. What does a Junior Security Analyst do?

In a Security Operations Center (SOC), a Junior Security Analyst operates primarily as a **Triage Specialist** (Tier 1):
- Continuous monitoring of SIEM alarms and security alerts.
- Validating whether alerts are false positives or true positives.
- Gathering initial triage artifacts (IPs, hashes, domain names, logs).
- Escalating verified security incidents to Tier 2 (Incident Responders) or SOC leadership.

---

## 2. Interactive Triage Simulation

1. Click **View Site** to open the SOC dashboard.
2. Review the incoming alerts to identify unauthorized external connection attempts.
3. The offending external IP generating malicious connection requests is:
   `221.181.185.159`
4. Following the organizational escalation procedure, escalate the ticket to the senior security lead:
   **Will Griffin**
5. Add a firewall drop rule for `221.181.185.159`. Once enforced, the malicious connection drops and logs the response flag:
   `THM{UNTIL-WE-MEET-AGAIN}`

---

## 🏁 Questions & Answers

| Task | Question | Answer |
|---|---|---|
| **Task 1** | What will be your role as a Junior Security Analyst? | `Triage Specialist` |
| **Task 3** | Malicious IP address found in alerts | `221.181.185.159` |
| **Task 3** | Person to whom the event was escalated | `Will Griffin` |
| **Task 3** | Message left by the attacker after blocking the IP | `THM{UNTIL-WE-MEET-AGAIN}` |
