# MD2PDF — TryHackMe

Room: https://tryhackme.com/room/md2pdf

MD2PDF is a web challenge built around a Markdown-to-PDF converter. The important question is what happens behind the interface when the server turns Markdown into a PDF. A feature that looks like simple text formatting can become a security issue if the rendering process can access resources it should not.

## Initial enumeration

After starting the machine, I use the IP address assigned by TryHackMe:

```bash
nmap -sV -sC -Pn -oN md2pdf-scan.txt TARGET_IP
```

This checks the target's exposed services, attempts version detection, runs the default scripts, and saves the output. I then open the web service in a browser and inspect the converter's normal behaviour before testing anything unusual.

## Understand the conversion feature

I start with harmless Markdown to confirm how the application accepts input and returns a PDF. Then I consider where rendering happens and whether the conversion engine can make server-side requests. This is the kind of trust boundary that can lead to server-side request forgery (SSRF) if a renderer is allowed to fetch URLs supplied by an untrusted user.

The exact test should follow the room's current instructions and remain limited to the deployed machine. I should not assume that every Markdown renderer is vulnerable or try to access unrelated internal services.

## What I took from this room

A file-conversion feature can create an unexpected path from user input to server-side network access. I need to understand the application's processing flow, validate any suspected SSRF carefully, and recommend restricting outbound requests and using a safer rendering configuration. The exact flag should be redacted as `flag{hidden}` in a public write-up after I verify the task result.