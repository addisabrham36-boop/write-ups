# OWASP Top 10 2025: Security Misconfiguration

Room: https://tryhackme.com/room/owasptopten2025

Security misconfiguration happens when a system is deployed with unsafe settings, unnecessary features, default credentials, overly broad permissions, or exposed debugging information. A secure application can become vulnerable because the environment around it was configured badly.

## What I look for

Examples include verbose error messages that reveal internals, unnecessary services, default accounts, directory listings, permissive cloud storage, and missing security headers. The relevant checks depend on the application and its hosting environment.

In a lab, I should verify the behaviour directly and record the evidence. A missing header may be worth fixing, but its impact is different from an exposed administrative interface with weak authentication.

## Prevention

Use hardened baseline configurations, remove unused components, change defaults, limit permissions, and review deployment settings. Configuration should be checked repeatedly because systems change over time. Error handling should help operators troubleshoot without exposing secrets to public users.

## What I took from this room

Security misconfiguration is often preventable through consistent deployment practices and regular review. The important habit is to check the actual running configuration rather than assume the secure setting in a template made it into production.