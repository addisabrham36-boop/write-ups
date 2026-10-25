# Hydra — TryHackMe

Room: https://tryhackme.com/room/hydra

Hydra is a login-testing tool that can try candidate usernames and passwords against supported services. In a training lab, it helps demonstrate why weak credentials and exposed authentication services can be risky. It should only be used against targets where testing is explicitly authorised.

## Understand the target first

Before using Hydra, I need to know which service the room asks me to test, the target address, and the expected username or wordlist. I should not assume that SSH is the target just because it is a common example. Different services use different syntax and options.

A generic SSH example is:

```bash
hydra -l USERNAME -P /path/to/wordlist.txt ssh://TARGET_IP
```

Here, `-l` sets one username, `-P` supplies a password list, and the final part identifies the SSH target. I replace all placeholders with values provided by the authorised lab. This example is not a claim that the room uses SSH or that a particular password will be found.

## Read the output carefully

A tool may report a credential pair that appears to work, but I should validate it only through the lab's intended login flow. Network errors, service limits, and incorrect options can cause failures that have nothing to do with the password being wrong. High request rates can also trigger account lockouts.

## Defending against password guessing

Long, unique passwords, multifactor authentication, sensible rate limiting, monitoring, and secure remote-access configuration all reduce risk. Rate limiting must be designed carefully so an attacker cannot easily lock out legitimate users.

## What I took from this room

Hydra automates login attempts, but it does not replace understanding the service or the task. I should use the exact target and method given by the room, explain each option, and record only results I actually observed.