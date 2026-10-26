# Advent of Cyber 2024 - Day 4: Atomic Red Team

{% embed url="https://tryhackme.com/room/adventofcyber2024" %}

I'm all atomic inside — let's simulate adversary TTPs and validate our detection coverage against MITRE ATT&CK.

## Background

Atomic Red Team is an open-source library of small, focused test cases (called "atomics") each mapped to a specific MITRE ATT&CK technique. The idea is simple: instead of waiting for a real attacker to test your defences, you simulate their exact behaviours yourself in a controlled way and verify whether your SIEM/EDR detects them.

Each atomic is identified by a technique ID from ATT&CK (e.g., `T1566.001` for Spearphishing via Attachment). The PowerShell module `Invoke-AtomicTest` is the primary runner. It can show details about a test before running it, execute the simulation, and clean up afterwards — all from a single command.

By running these simulations and checking whether alerts fire, defenders can measure detection gaps. If `T1566.001` runs silently with no alert, your phishing attachment detection is broken and needs tuning. This "purple team" approach — combining red team simulation with blue team validation — dramatically improves detection posture without waiting for a real breach.

## Walkthrough

**Step 1 — Explore available atomics for a technique**

Let's look at what test cases are available for spearphishing via attachment (`T1566.001`):

```powershell
Invoke-AtomicTest T1566.001 -ShowDetails
```

This lists all available tests, their descriptions, required dependencies, and the commands they'll run. Review these before executing.

**Step 2 — Run the phishing simulation**

```powershell
Invoke-AtomicTest T1566.001
```

This simulates a spearphishing attachment being opened. Watch your SIEM/EDR for alerts — did it fire?

**Step 3 — Clean up after the test**

```powershell
Invoke-AtomicTest T1566.001 -Cleanup
```

Always clean up after atomics to restore the system to a known-good state.

**Step 4 — Verify detection**

Check Elastic/Kibana or your SIEM for alerts generated during the test. If nothing fired, that's a detection gap worth investigating.

**Step 5 — Explore other techniques as needed**

The same pattern applies to any ATT&CK technique ID. Browse the full list at [github.com/redcanaryco/atomic-red-team](https://github.com/redcanaryco/atomic-red-team).

```powershell
# Example: test credential dumping
Invoke-AtomicTest T1003.001 -ShowDetails

# Example: test scheduled task persistence
Invoke-AtomicTest T1053.005
```

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| What command runs the spearphishing via attachment simulation? | `Invoke-AtomicTest T1566.001` |
