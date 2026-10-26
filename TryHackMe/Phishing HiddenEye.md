# Phishing: HiddenEye

{% embed url="https://tryhackme.com/room/phishinghiddeneye" %}

An introduction to modern automated phishing toolchains and credential harvesting infrastructure using HiddenEye, demonstrating clone generation, tunneling, and defensive countermeasures.

---

## 1. What is HiddenEye?

**HiddenEye** is an open-source, modular phishing framework written in **Python**. It automates the generation of credential-harvesting phishing templates mimicking popular social networks, email providers, and cloud services.

Key Capabilities:
- Cloning login forms with high visual fidelity.
- Optional keylogger integration to capture keystrokes prior to form submission.
- Automated reverse tunneling (via Localhost, Ngrok, or Serveo) to expose local test servers to external test recipients.

---

## 2. Tool Execution Workflow

In an authorized security awareness simulation:

```bash
cd HiddenEye
python3 HiddenEye.py
```

The menu prompts:
1. Select target platform template (e.g. Google, Microsoft, Facebook).
2. Choose auxiliary features (enable/disable Keylogger, Cloudflare anti-bot landing disguise).
3. Select port forwarding / tunneling provider.
4. Distribute the generated test link and monitor console logs for incoming POST submissions.

---

## 3. Defense & Prevention

- **FIDO2 / WebAuthn Hardware Keys:** Hardware MFA binds authentication to the true cryptographic domain origin (origin binding), rendering credential harvesting completely ineffective even if the user falls for the fake URL.
- **DMARC Enforcement:** Blocks spoofed emails pretending to originate from authentic organizational mail servers.
- **URL Inspection:** Training staff to inspect the effective Top-Level Domain (e.g. verifying `paypal.com` vs `paypal.com.login-verify.xyz`).

---

## 🏁 Questions & Answers

| Task | Question | Answer |
|---|---|---|
| **Task 2** | Type of attack targeting human psychology instead of technical code | `Social engineering` |
| **Task 3** | Programming language HiddenEye is written in | `Python` |
| **Task 4** | Does HiddenEye support adding a keylogger to track victim inputs? | `Yes` |
| **Defensive** | Most effective countermeasure against credential harvesting | FIDO2 / WebAuthn hardware-bound MFA |
