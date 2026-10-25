# Phishing: HiddenEye — TryHackMe Walkthrough

Official room: https://tryhackme.com/room/phishinghiddeneye

## Room Overview & Information

**Category:** Social engineering and phishing awareness  
**Difficulty:** Beginner  
**Tasks:** Installation and first phishing-page demonstration, link masking, and a Gmail-themed example.

HiddenEye is a phishing simulation tool that can generate lookalike login pages. The room teaches how these pages exploit user trust and why a realistic-looking page or HTTPS connection is not proof that a website is legitimate. Use the tool only in the room's authorised environment. Do not send phishing links to real people or collect real credentials.

## Task 1: Installation & Creating Your First Phishing Page

Follow the room's current instructions for the provided AttackBox or lab environment. Tool dependencies and repository installation steps can change, so use the commands on the active room page rather than installing an unverified copy from an unrelated source.

The goal of this task is to observe how a phishing simulation can imitate a legitimate login page. Compare the simulated page with the real service's known domain and expected sign-in flow. Do not enter a real password or use the simulated page outside the lab.

### Question: “Which image shows a legit web-page? They are identical and most phishing pages nowadays have HTTPS enabled. (Image 1 or Image 2)”

**Answer: Image 2.**

The task highlights that the two pages can look identical. The domain and the context of the link matter more than visual design alone. HTTPS encrypts the connection to the domain in the address bar; it does not prove that the domain belongs to the organisation being impersonated.

### Question: “What will you use this tool for?”

**Answer: Educational Purposes.**

The tool is intended here for a controlled learning exercise. A phishing toolkit must not be used to deceive real users, collect credentials, or access accounts without permission.

## Task 2: Masking Your Link

Link masking attempts to make a destination appear more trustworthy than it is. A message may display a familiar brand name while the actual URL points to an unrelated domain or a redirect service.

When reviewing a link:
1. Hover over it or inspect the link address without opening it.
2. Read the hostname carefully.
3. Identify the registered domain, reading from right to left.
4. Check for misspellings, extra subdomains, URL shorteners, and unexpected redirects.
5. If the message claims to be from a service you use, open that service using a known bookmark or manually entered address instead.

A domain such as `accounts.example.com.attacker.test` belongs to `attacker.test`, not `example.com`. A lock icon does not make a deceptive domain legitimate.

## Task 3: Creating a Gmail Phishing Page

The room demonstrates a Gmail-themed lookalike page to show how attackers can abuse familiar branding. The important learning objective is to understand the risk and recognise the indicators, not to deploy a credential-harvesting page.

Compare the simulated page with the legitimate sign-in process:
- Is the hostname exactly the expected Google domain?
- Was the link received unexpectedly?
- Does the page request credentials after an unusual prompt?
- Does the link redirect through an unrelated service?
- Is the user being pressured to act immediately?

Do not submit real Google credentials to a simulation. Use only the test environment and accounts explicitly provided by the room.

### Question: “What is the weakest link in cyber-security?”

**Answer: Humans.**

This is the room's intended awareness answer: social engineering targets human judgement and trust. The phrase does not mean users are to blame for every incident. Good security design assumes people can be deceived and adds controls that limit the impact.

### Question: “Do most phishing pages have HTTPS (Yay/Nay)?”

**Answer: Yay.**

Many phishing pages use HTTPS. A valid TLS connection means traffic is encrypted to the domain in the browser; it does not establish that the site is genuine. Always verify the hostname and the context of the link.

## Evidence and interpretation

The exercise shows four key ideas:
1. A phishing page can closely imitate a legitimate page.
2. Link masking can make the destination less obvious.
3. Humans are frequent targets of social engineering.
4. HTTPS is not a reliable test of whether a page is trustworthy.

The strongest verification method is to inspect the actual domain and navigate to the service through a known, trusted route.

## Remediation and prevention

- Use phishing-resistant MFA where possible.
- Use a password manager that recognises the legitimate domain.
- Train users to report suspicious messages without fear of blame.
- Deploy email authentication and filtering.
- Monitor for unusual authentication attempts and new forwarding rules.
- Use unique passwords for every service.
- Provide a quick process for revoking sessions and rotating exposed credentials.
- Keep phishing simulations scoped, approved, and limited to test accounts.

## Key takeaways

HiddenEye demonstrates how easy it can be to imitate a familiar login page. The defensive lesson is to verify the destination, not just the page's appearance or HTTPS status, and to use phishing tools only for authorised education and testing.