# Passive Reconnaissance

{% embed url="https://tryhackme.com/room/passiverecon" %}

Passive recon = gathering intel without directly touching the target. WHOIS, DNS lookups, Shodan, DNSDumpster. No packets sent to the target.

---

## Passive vs Active

| Scenario | Type |
|---|---|
| Visit the target company's Facebook page for employee names | **P** (Passive) |
| Ping the target webserver to check ICMP | **A** (Active) |
| Social engineer the IT admin at a party | **A** (Active) |

---

## WHOIS

```bash
whois tryhackme.com
```

| Question | Answer |
|---|---|
| When was TryHackMe.com registered? | `20180705` |
| What is the registrar? | `namecheap.com` |
| Which company for name servers? | `cloudflare.com` |

---

## nslookup & dig

Look up TXT records:

```bash
dig thmlabs.com TXT
# or
nslookup -type=TXT thmlabs.com
```

**Flag in TXT record:** `THM{a5b83929888ed36acb0272971e438d78}`

---

## DNSDumpster

Look up `tryhackme.com` on [dnsdumpster.com](https://dnsdumpster.com). Beyond `www` and `blog`, there's an interesting subdomain:

**Answer:** `remote`

---

## Shodan.io

Shodan indexes exposed devices. As of the room's data, the first country by exposed devices is:

**Answer:** `US`

---

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| Facebook page visit — passive or active? | `P` |
| Pinging a server — passive or active? | `A` |
| Social engineering at a party — passive or active? | `A` |
| When was TryHackMe.com registered? | `20180705` |
| What is the registrar of TryHackMe.com? | `namecheap.com` |
| Name server company for TryHackMe.com? | `cloudflare.com` |
| Flag in TXT records of `thmlabs.com` | `THM{a5b83929888ed36acb0272971e438d78}` |
| Interesting subdomain found on DNSDumpster? | `remote` |
| First country in exposed Shodan devices? | `US` |
