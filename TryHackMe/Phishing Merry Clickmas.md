# Phishing: Merry Clickmas — TryHackMe Walkthrough

Official room: https://tryhackme.com/room/phishing-aoc2025-h2tkye9fzU

## Room Overview & Information

**Event:** Advent of Cyber 2025, Day 2  
**Category:** Social engineering / email security  
**Focus:** Creating a controlled phishing simulation, analysing a harvested test credential, and checking for password reuse in a simulated mailbox.

The exercise uses the fictional TBFC environment. It demonstrates how a convincing operational email can persuade a recipient to visit a lookalike page, and how password reuse can turn one compromised account into access to another service. Perform these steps only inside the room's lab. Do not send phishing email to real recipients or collect real credentials.

## Phase 1: Understand the scenario

The scenario asks you to send a phishing message to the fictional factory mailbox. The email claims that the shipping schedule has changed and asks the recipient to confirm the new schedule using a lab-hosted page. The purpose is to demonstrate how a plausible work-related request can bypass a person's caution.

The key learning objectives are:
- Understand how social engineering creates urgency or operational pressure.
- Recognise how a lookalike page can collect a test credential.
- Confirm whether a password has been reused on another service.
- Identify practical controls that prevent this chain from succeeding.

## Phase 2: Start the lab and launch the supplied page

Start the room's AttackBox and target environment. Use the script supplied by the task to start the simulated page. Run it from the directory where the room's file is stored:

```bash
./server.py
```

If execution is denied, check the file permissions and follow the room's instructions for running the provided script. Do not replace it with a public phishing kit or host it outside the lab.

Open the URL and port specified by the active room to confirm that the training page is reachable. Use the actual lab address rather than copying a temporary IP from another walkthrough.

## Phase 3: Prepare the simulated email with SET

The room uses the Social-Engineer Toolkit (SET) to send a single training email. Start SET in the AttackBox using the command supplied by the environment. In the menu, select the options indicated by the task:

1. **Social-Engineering Attacks**
2. **Mass Mailer Attack**
3. **E-Mail Attack Single Email Address**
4. **One-Time Use Email Template**

The names and menu numbers can vary by SET version. Read the menu on your screen and select the matching options rather than assuming the numbers are unchanged.

The room's fictional email uses the subject **Shipping Schedule Changes**, a plain-text body, and the fictional recipient `factory@wareville.thm`. The message claims the schedule has changed and directs the recipient to the lab page. Use the sender and SMTP settings displayed by the active room. They are part of the isolated training environment and must not be replaced with a real mail server or real recipient.

Before sending, verify:
- The recipient is the fictional address specified by the room.
- The message links only to the lab-hosted page.
- No real password or personal data is included.
- The SMTP server and sender are the lab's provided values.
- The exercise is being run within the authorised environment.

Send the single test email and wait for the lab's notification that the recipient interacted with the page.

## Phase 4: Retrieve the test credential

The room's simulated recipient follows the link, and the training environment records the credential submitted to the page. Use the notification or result displayed by the room to obtain the password. Do not treat a successful notification as proof that a real user was compromised; this is a controlled simulation.

### Question 1: “What is the password used to access the TBFC portal?”

The lab's simulated phishing interaction reveals the test password:

**Answer:** `unranked-wisdom-anthem`

This is a credential for the fictional TryHackMe exercise only. Never reuse it outside the lab.

## Phase 5: Test for password reuse

The next task asks you to open the factory mailbox and determine whether the harvested password was reused. In a real incident, password reuse is a serious risk because a credential exposed on one service may work on email, VPN, or another business application.

Browse to the mailbox service at the target address shown by the room:

```text
http://MACHINE_IP
```

Replace MACHINE_IP with the active target address. Sign in with the fictional factory account details supplied by the room and the password retrieved in the previous task. Do not test these credentials against any other system.

Once logged in, inspect the received messages. Locate the email containing the shipment information and read the total number of toys expected for delivery. Make sure you are reading the total requested by the question, not an individual shipment or a message timestamp.

### Question 2: “What is the total number of toys expected for delivery?”

**Answer:** **1984000**

The value is taken from the message in the simulated factory mailbox. The lesson is that a phishing compromise may become more serious when a user reuses the same password across services.

## Phase 6: Evidence and interpretation

The chain is:
1. A believable email is sent to the fictional recipient.
2. The recipient visits the training page.
3. The lab records the test credential.
4. The same credential is accepted by the mailbox service.
5. The mailbox contains operational information that was not available through the initial page.

Each step proves a different point. A delivered email does not prove that it was opened. A page visit does not prove that a credential was submitted. A captured test credential does not prove access to another service until the second login succeeds.

## Phase 7: Mitigation and prevention

- Use unique passwords for every service and a password manager.
- Enable MFA, preferably phishing-resistant authentication for important accounts.
- Use email authentication controls and secure email gateways.
- Provide a simple way to report suspicious messages.
- Verify urgent operational changes through a known independent channel.
- Restrict mailbox access and monitor unusual sign-ins.
- Train staff to recognise unexpected links and credential requests.
- Use lab-only accounts and controlled recipients for authorised phishing simulations.
- Rotate credentials and revoke sessions quickly if a real credential is exposed.

## Key takeaways

Phishing works by making a request appear routine and urgent. This room shows how a simulated email can lead to a test credential and how password reuse can extend the impact to a mailbox. Unique passwords, MFA, independent verification, and quick reporting reduce the risk.

## Phase 8: Room Questions & Answer Summary

| Exact question | Answer |
|---|---|
| What is the password used to access the TBFC portal? | `unranked-wisdom-anthem` |
| What is the total number of toys expected for delivery? | `1984000` |
| What's the flag? | Retrieve it from the active room after completing the simulation; public copy: `THM{hidden}` |

The password and shipment total are documented values from the standard exercise, but the active room takes precedence if the task changes. The flag is generated by the interactive room and is not guessed or replaced with a fabricated value.

## Phase 9: Evidence and Remediation Checklist

Preserve the simulated email details, actual link destination, lab credential result, successful mailbox login, and message containing the shipment total. Separate visiting the training page from credential submission, and separate credential capture from successful credential reuse.

Use unique passwords, password managers, phishing-resistant MFA, email authentication, user reporting, and independent verification of urgent operational requests. Keep simulations approved and restricted to test recipients and lab-only credentials.
