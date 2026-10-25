# Passive Reconnaissance: TryHackMe Walkthrough

Official room: https://tryhackme.com/room/passiverecon

## Room Overview & Information

**Category:** OSINT and reconnaissance  
**Difficulty:** Beginner  
**Focus:** Passive versus active reconnaissance, WHOIS, DNS lookups, DNSDumpster, and Shodan.

Passive reconnaissance collects information from existing public sources without directly probing the target's services. Active reconnaissance interacts with the target, for example by pinging a host or scanning its ports. This room uses public information and a simulated DNS lab to teach the difference.

## Task 2: Passive Versus Active Recon

### Question: “You visit the Facebook page of the target company, hoping to get some of their employee names. What kind of reconnaissance activity is this?”

**Answer: P (Passive).**

The researcher is looking at information already published on a third-party social platform and is not directly probing the target's systems.

### Question: “You ping the IP address of the company webserver to check if ICMP traffic is blocked. What kind of reconnaissance activity is this?”

**Answer: A (Active).**

Ping sends network traffic to the target and observes the response. It can appear in logs and is therefore active reconnaissance.

### Question: “You happen to meet the IT administrator of the target company at a party. You try to use social engineering to get more information about their systems and network infrastructure. What kind of reconnaissance activity is this?”

**Answer: A (Active).**

This is direct interaction with a person to obtain information. The room classifies this as active reconnaissance because the researcher is directly engaging the target organisation. In a real engagement, social engineering must be explicitly included in the written scope.

## Task 3: WHOIS

WHOIS returns domain-registration details such as registrar, registration dates, and nameservers. Some fields may be privacy-protected, and results vary by registry and date.

Run:

\`\`\`bash
whois tryhackme.com
\`\`\`

Read the relevant field rather than relying on a search snippet.

### Question: “When was TryHackMe.com registered?”

**Room answer:** `20180705`.

This is the date format used by the room's WHOIS result: YYYYMMDD.

### Question: “What is the registrar of TryHackMe.com?”

**Room answer:** `namecheap.com`.

### Question: “Which company is TryHackMe.com using for name servers?”

**Room answer:** `CLOUDFLARE.COM`.

WHOIS data can change. If the current room or live lookup returns a different value, follow the active task's data and record the query date.

## Task 4: nslookup and dig

DNS maps names to records. Different record types answer different questions:
- **A:** IPv4 address.
- **AAAA:** IPv6 address.
- **MX:** mail exchanger and preference.
- **NS:** authoritative nameserver.
- **TXT:** text data, often used for verification or email policy.

The room asks you to query the TXT record for thmlabs.com. Use either tool:

\`\`\`bash
nslookup -type=TXT thmlabs.com
dig thmlabs.com TXT
\`\`\`

- **nslookup -type=TXT** asks for TXT records.
- **dig thmlabs.com TXT** performs the same query using dig.
- The answer section contains the record value.

### Question: “What is the TXT record value for thmlabs.com?”

The record contains a flag. Obtain it from the active room's DNS result and submit it there. The flag is deliberately redacted in this public write-up as `THM{hidden}`.

A DNS record is public data, not encrypted secret storage. Do not place credentials or private keys in TXT records.

## Task 5: DNSDumpster

DNSDumpster aggregates publicly available DNS and related information. Search for `tryhackme.com` and inspect the subdomains listed in the result. The question asks for an interesting subdomain in addition to `www` and `blog`.

### Question: “Lookup tryhackme.com on DNSDumpster. What is one interesting subdomain that you would discover in addition to www and blog?”

**Room answer:** `remote`.

DNSDumpster results can change over time. Use the active room's expected result if the current site presents different data, and do not assume that a discovered hostname is currently online or vulnerable.

## Task 6: Shodan.io

Shodan indexes publicly reachable services observed by its crawlers. Search results include service banners, ports, and other metadata. They represent observations at a point in time and should not be treated as proof that a service is still available.

### Question: “According to Shodan.io, what is the 2nd country in the world in terms of the number of publicly accessible Apache servers?”

**Room answer:** **Germany**.

### Question: “Based on Shodan.io, what is the 3rd most common port used for Apache?”

**Room answer:** **8080**.

### Question: “Based on Shodan.io, what is the 3rd most common port used for nginx?”

**Room answer:** **8888**.

Shodan's rankings can change as it scans new systems and changes its classification. These are the room's expected answers, not permanent global facts. If the room's live dashboard differs, follow its current result.

## Reporting and responsible use

For every discovery, record the source, query, date, observation, and confidence. A certificate record proves a hostname appeared in a certificate; it does not prove the host is currently online. A Shodan result shows an observed service, not necessarily a current vulnerability.

Do not attempt to access or exploit discovered systems unless they are within the authorised scope. Publicly indexed information can still be sensitive, so minimise collection and report accidental exposure responsibly.

## Key takeaways

Passive reconnaissance uses public information; active reconnaissance interacts directly with a target. WHOIS provides registration context, DNS queries return record data, DNSDumpster helps discover subdomains, and Shodan summarises observed internet-facing services. Verify results, record when they were obtained, and avoid turning a clue into an unsupported conclusion.

## Expanded practical workflow

### 1. Define the target and passive boundary

Passive reconnaissance collects information from public or already available sources without directly probing the target infrastructure. Confirm the target organization or domain from the room and record the scope. Do not assume that a public subdomain or third-party service is authorized for active testing.

### 2. Registration and DNS information

WHOIS or registrar records can provide registration dates, nameservers, and contact or organization data, depending on privacy controls. DNS records reveal how a domain is configured. Query only the records needed for the task:

```bash
dig TARGET_DOMAIN A
dig TARGET_DOMAIN AAAA
dig TARGET_DOMAIN MX
dig TARGET_DOMAIN NS
dig TARGET_DOMAIN TXT
```

Replace TARGET_DOMAIN with the exact domain from the room. A records map names to IPv4 addresses, AAAA records to IPv6 addresses, MX records identify mail exchangers, NS records identify authoritative nameservers, and TXT records can include verification or email-security policies.

DNS answers may be cached or change over time. Record the query time and resolver if the exact result matters.

### 3. Certificate transparency and subdomains

Certificate-transparency logs show certificates that have been issued and may reveal hostnames. Search only the room's target domain and record candidate subdomains. A name in a certificate log does not prove that the service is still online or belongs to the target today.

Use public search engines, official documentation, and the room's specified services to collect the requested clues. Do not brute-force DNS or probe candidate hosts unless the room explicitly authorizes active enumeration.

### 4. Public documents and metadata

Public PDFs, office documents, repositories, and job postings can reveal software names, usernames, internal terminology, or metadata. Treat this information as a lead, not proof of a vulnerability. Avoid collecting personal information that is not relevant to the exercise.

If the room asks for a particular record or value, save the source URL and verify the exact field rather than relying on a search-result snippet alone.

### 5. Organize the findings

Maintain an inventory with the asset, source, timestamp, observed value, confidence, and next step. Mark facts separately from assumptions. Passive data can be stale or misleading, so validate important findings before using them in a security assessment.

### 6. Answering the room tasks

For each question, identify the public source the room expects, navigate to the relevant record, and capture the exact value. If the question asks for a domain, mail server, nameserver, or organization detail, use the correct DNS or registration record type. Do not substitute an IP or unrelated hostname because it looks plausible.

### 7. Ethical and defensive use

Passive recon helps defenders understand their external attack surface. Remove abandoned DNS records, avoid exposing internal names in public documents, monitor certificate issuance, and keep public repositories free of secrets. If a secret is exposed, rotate it rather than simply deleting the public copy.
