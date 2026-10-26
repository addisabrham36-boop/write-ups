# Advent of Cyber 2024 - Day 14: Certificate MitM

{% embed url="https://tryhackme.com/room/adventofcyber2024" %}

Certificate of Authenticity

## Background

Transport Layer Security (TLS) protects data in transit by providing encryption, integrity, and server authentication through Public Key Infrastructure (PKI). Intercepting HTTPS traffic requires positioning an inspecting proxy between client and server (Man-in-the-Middle).

To inspect encrypted traffic, testing proxies like Burp Suite generate dynamic leaf certificates signed by their own custom Certificate Authority (CA). Unless an application implements strict certificate pinning, trusting this root CA allows full decryption and payload tampering.

## Walkthrough

**Step 1 — Configure the CA Certificate**

Export the Burp Suite root certificate (`cacert.der`) and convert to PEM format if needed:
```bash
openssl x509 -inform DER -in cacert.der -out cacert.pem
```
Install the certificate into the victim client's trust store.

**Step 2 — Route Traffic Through Proxy**

Set client proxy configuration to `127.0.0.1:8080`. Launch the targeted desktop application.

**Step 3 — Inspect Decrypted HTTPS Requests**

Burp intercepts the application's outbound API calls. Modify the authorization payload in transit:
```http
POST /api/license/verify HTTP/1.1
Host: license.wareville.thm
Authorization: Bearer <tampered_token>
```
The server accepts the modified request and returns the challenge flag.

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| What file format does Burp Suite export its CA certificate in by default? | `DER` |
| What security mechanism binds an application strictly to a specific certificate? | `Certificate Pinning` |
| What is the flag found after inspecting the decrypted traffic? | `THM{AoC-h0wt0ru1nG1ftD4y}` |
