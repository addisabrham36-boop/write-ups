# Passive Reconnaissance: Notes

This is a supplementary note for passive-recon concepts, not a replacement for the main room write-up.

Passive reconnaissance uses information available from public sources, such as DNS records, search results, certificate-transparency data, public documents, and registration records. It is useful because it can build a picture of an organisation without directly probing its infrastructure.

## Quick reference

```bash
dig example.com A
dig example.com MX
dig example.com NS
whois example.com
```

These are generic examples. I should replace the example domain only with an authorised assessment target. DNS and WHOIS data may be incomplete, cached, privacy-protected, or out of date.

The important habit is to record the source and date of a finding and distinguish confirmed facts from assumptions. Publicly visible does not mean permission has been granted to misuse the information.