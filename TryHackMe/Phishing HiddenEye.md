# Phishing: HiddenEye — Complete TryHackMe Walkthrough

Official room: https://tryhackme.com/room/phishinghiddeneye

## Room Overview & Information

**Category:** Social engineering and phishing awareness  
**Difficulty:** Beginner  
**Topics:** Lookalike pages, URL masking, HTTPS misconceptions, and ethical phishing simulation.

HiddenEye is a phishing simulation tool capable of creating pages that imitate popular login interfaces. This room demonstrates how a page can look legitimate while the destination is controlled by someone else. Keep all actions in the provided training environment, use dummy accounts, and do not collect or retain real credentials.

## Phase 1: Reconnaissance & Enumeration

### Task 1: Installation and first phishing-page demonstration

Follow the active room's setup instructions for the provided AttackBox. Dependencies and repository installation steps can change, so use the official room's current instructions rather than installing an unverified copy from an unrelated source.

Open the generated training page and compare it with the legitimate service's sign-in page. Visual similarity is a clue attackers exploit, not proof that the destination is authentic. Inspect the actual hostname and link path. Do not enter a real password into the simulation.

### Question: “Which image shows a legit web-page? They are identical and most phishing pages nowadays have HTTPS enabled. (Image 1 or Image 2)”

**Answer:** Image 2.

The intended lesson is to validate the destination domain and context rather than trust the page's appearance. HTTPS encrypts the connection to the domain in the address bar; it does not guarantee that the domain belongs to the organisation being impersonated.

### Question: “What will you use this tool for?”

**Answer:** Educational Purposes.

Use the tool only for controlled learning or other explicitly authorised testing. Do not deceive real users, harvest credentials, or use captured information to access accounts.

## Phase 2: Vulnerability Analysis & Link Validation

### Task 2: Masking your link

A phishing message can make a link appear familiar while directing the browser to an unrelated hostname. To inspect a URL safely:

1. Hover over the link or inspect its underlying href without opening it.
2. Read the hostname carefully.
3. Identify the registered domain, not just a familiar word earlier in the URL.
4. Check for misspellings, added subdomains, URL shorteners, and redirects.
5. Navigate to a sensitive service through a known bookmark or manually entered address instead of clicking an unexpected link.

For example, accounts.example.com.attacker.test is hosted under attacker.test, not example.com. A valid certificate only shows that the connection is encrypted to the named domain.

### Task 3: Gmail-themed simulation

The room uses a Gmail-themed page to demonstrate the persuasive effect of familiar branding. Compare the URL with the real service, inspect the message that led to the page, and identify whether it asks for credentials under an unusual pretext. Do not submit genuine Google credentials. The room is testing recognition and ethical use, not permission to deploy a credential-harvesting page.

### Question: “What is the weakest link in cyber-security?”

**Answer:** Humans.

This is the intended awareness answer because social engineering targets human trust and decision-making. It does not mean that people should be blamed for every incident. Effective security assumes that users can be deceived and limits the damage through authentication, access control, filtering, and clear reporting processes.

### Question: “Do most phishing pages have HTTPS (Yay/Nay)?”

**Answer:** Yay.

Phishing sites often use HTTPS. TLS protects traffic between the browser and the named domain; it does not validate the domain's business identity. Check the hostname and the source of the link.

## Phase 3: Post-Exercise Investigation

No privileged shell, administrator role, or host compromise is part of the standard awareness tasks. The relevant post-exercise work is to understand what evidence would exist in a real phishing investigation:
- Email sender and authentication headers.
- Real URL, redirect chain, and domain registration context.
- Web server and DNS logs.
- Authentication logs showing whether credentials were submitted.
- Suspicious mailbox rules, session grants, or endpoint activity after a click.

If real credentials were accidentally entered into a phishing page, change them through the legitimate service, revoke active sessions, review account activity, and report the incident. Do not reuse the exposed password.

## Phase 4: Room Questions & Answers

| Exact question | Answer |
|---|---|
| Which image shows a legit web-page? (Image 1 or Image 2) | Image 2 |
| What will you use this tool for? | Educational Purposes |
| What is the weakest link in cyber-security? | Humans |
| Do most phishing pages have HTTPS (Yay/Nay)? | Yay |

## Phase 5: Key Takeaways & Remediation

- Verify hostnames instead of judging sites by branding alone.
- Use a password manager that recognises the legitimate domain.
- Prefer phishing-resistant MFA.
- Filter suspicious email and inspect domains/redirects.
- Provide a safe way for users to report suspected phishing without fear of blame.
- Monitor suspicious authentication, mailbox changes, and unusual sessions.
- Keep phishing simulations approved and limited to test users and training infrastructure.

The central lesson is that a phishing page can look polished and use HTTPS while still being fraudulent. The destination and the context matter more than the page's visual appearance.