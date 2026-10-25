# Phishing with HiddenEye — TryHackMe

Room: https://tryhackme.com/room/phishinghiddeneye

This topic demonstrates how phishing pages can imitate legitimate login screens and trick people into entering credentials. Tools that generate imitation pages are dangerous when used against real people, so I should keep any practice inside the explicitly authorised lab and never collect real credentials.

## How the attack works

A phishing page may copy the appearance of a trusted service and rely on a deceptive link to bring a victim to it. The page can look convincing while the domain is unrelated to the real provider. Users should verify the domain and avoid entering credentials after following unexpected links.

## Defensive checks

Use multifactor authentication, password managers that check the site origin, phishing-resistant authentication where available, and clear reporting procedures. Security awareness helps, but organisations should also use technical controls and minimise the damage a stolen password can cause.

## Lab notes

I should follow the room's instructions and use only the provided test environment. I should not deploy a phishing page publicly, send it to unsuspecting users, or retain any credentials. If a task asks for a result, I should record the evidence and redact secrets in the public write-up.

## What I took from this room

Phishing succeeds by exploiting trust and convincing presentation. The most important defence is to verify the destination and use authentication controls that reduce the value of a stolen password.