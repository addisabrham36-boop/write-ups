# MD2PDF: TryHackMe Walkthrough

Official room: https://tryhackme.com/room/md2pdf

## Room Overview & Information

**Category:** Web exploitation  
**Difficulty:** Easy  
**Primary vulnerability:** Server-Side Request Forgery (SSRF) through HTML accepted by a Markdown-to-PDF converter.  
**Goal:** Retrieve the flag from an internal administrative page that is not directly accessible from the outside.

MD2PDF accepts Markdown and renders a PDF on the server. The key security issue is that the rendering process handles raw HTML and can fetch referenced resources. If the renderer can access internal services, a user may be able to make the server request content that the external browser cannot reach.

## Phase 1: Reconnaissance

Start the lab machine and record the assigned IP. Scan the target:

```bash
nmap -sC -sV -oN md2pdf-nmap.txt MACHINE_IP
```

- `-sC` runs Nmap's default scripts.
- `-sV` attempts service-version detection.
- `-oN` saves the normal report.
- `MACHINE_IP` is the current address supplied by TryHackMe.

The intended target exposes a public web application and a second web service on port 5000. Confirm the actual scan results from your instance; do not reuse another user's IP or assume the service versions are identical.

Open the main web service in the browser. It provides a Markdown input and a PDF conversion function. First submit harmless Markdown, such as:

```markdown
# Test document

This is **bold text**.
```

Confirm that the application produces a PDF. This establishes the normal workflow before testing how HTML is handled.

## Phase 2: Enumerate web paths

Use a content-discovery tool against the authorised lab target:

```bash
gobuster dir -u http://MACHINE_IP/ -w /usr/share/wordlists/dirb/common.txt -o md2pdf-gobuster.txt
```

- `dir` performs directory and file discovery.
- `-u` sets the target URL.
- `-w` selects a wordlist.
- `-o` saves the results.

Use a wordlist that exists in your AttackBox; adjust the path if necessary. Review the results manually and investigate the `/admin` route. The room's admin endpoint responds with a forbidden message indicating it can only be accessed internally on `localhost:5000`.

Verify the response directly:

```bash
curl -i http://MACHINE_IP/admin
```

The `-i` option includes response headers. A 403 response is evidence of an access restriction, not evidence that the route is nonexistent.

## Phase 3: Identify the rendering trust boundary

The converter accepts Markdown, but Markdown renderers may also permit raw HTML. If the PDF renderer loads external resources, HTML elements such as `img`, `iframe`, or `object` can trigger requests during document generation.

This creates two distinct request origins:
1. The browser sends Markdown to the public converter.
2. The server-side renderer processes the Markdown and fetches referenced resources.

If the second request originates from the server, it may reach a service bound to localhost that an external client cannot access. This is the core SSRF trust-boundary failure.

## Phase 4: Test controlled HTML rendering

Use the room's converter and submit the following HTML as Markdown:

```html
<iframe src="http://localhost:5000/admin" width="1000" height="800"></iframe>
```

The URL points to the internal service from the PDF renderer's perspective. Generate the PDF and open the downloaded document. If the renderer fetches the resource and embeds its response, the internal admin page will appear in the PDF.

The important observation is not merely that the application accepts an iframe. It is that the server-side rendering process can fetch a localhost-only resource and include its content in the output.

If the page does not appear, verify:
- The correct target IP and public converter are being used.
- The URL uses the internal port specified by the lab.
- Raw HTML is being processed by the renderer.
- The conversion completed and the generated PDF is the newest output.

Do not substitute a flag from another instance.

## Phase 5: Retrieve and verify the flag

The internal admin content is rendered into the generated PDF. Open the PDF and locate the flag displayed by the lab. If text extraction is useful, install or use `pdftotext` in the AttackBox:

```bash
pdftotext generated.pdf -
```

- `pdftotext` extracts readable text from a PDF.
- `generated.pdf` is the downloaded file from your current run.
- `-` sends extracted text to standard output.

If the PDF is image-based or the content is not extractable, inspect the document visually. Submit the flag shown by the live lab. This public write-up intentionally uses `flag{hidden}` rather than publishing an unverified value.

## Phase 6: Explain the vulnerability chain

The complete chain is:

1. The application accepts user-controlled Markdown.
2. The Markdown renderer permits raw HTML or otherwise processes embedded resource references.
3. The PDF renderer makes a request to the URL embedded in the content.
4. The renderer runs on the server and can reach `localhost:5000`.
5. The internal admin service trusts localhost requests.
6. The response is embedded in the generated PDF.
7. The user reads information that should not have been exposed.

This is SSRF combined with unsafe HTML rendering. It is not simply a browser-side XSS issue because the sensitive request is made by the server-side renderer.

## Phase 7: Remediation

- Disable raw HTML in Markdown when it is not required.
- Sanitize content using a well-maintained allowlist-based HTML sanitizer.
- Configure the PDF renderer to disable local file access and external network requests where possible.
- Isolate the renderer in a sandbox with no access to internal administrative services.
- Block loopback, link-local, private, and metadata-service destinations at the network layer, while accounting for redirects and DNS rebinding.
- Bind administrative services to an appropriate interface and require authentication rather than trusting source address alone.
- Apply least privilege and keep the rendering library updated.
- Add regression tests that submit HTML references to internal destinations and confirm that they are rejected.

A URL filter alone is not enough if the renderer can follow redirects or resolve a hostname to a private address after validation. Enforce policy at the point where the network request is made.

## Key takeaways

MD2PDF demonstrates how a document-conversion feature can become a route into internal services. Enumerate the exposed endpoints, inspect the 403 response, test how the server renders HTML, and verify the internal response in the generated PDF. The fix is to restrict both the content that is rendered and the network access available to the renderer.

## Expanded exploitation analysis and validation

### 1. Trace the document-conversion pipeline

A Markdown-to-PDF service commonly accepts user text or a file, converts Markdown to HTML, then renders HTML into a PDF using a browser engine or conversion library. Each transition is a trust boundary. The risk depends on whether user-controlled content can invoke raw HTML, access local files, load remote URLs, or trigger dangerous renderer features.

Record the exact input field, request format, response, and conversion behaviour in the room. Start with a benign Markdown sample and compare it with a sample containing HTML that the application claims to sanitize. Do not infer a vulnerability merely because the output contains HTML-looking characters.

### 2. Inspect the HTTP interaction

Use browser Developer Tools or Burp Suite to capture the request. Note the method, path, Content-Type, parameters, cookies, and response. If the UI sends a JSON request, preserve the exact JSON structure. A mismatch in field name or encoding can produce a false negative.

### 3. Test trust boundaries with harmless content

First determine whether raw HTML is accepted, escaped, or removed. If the room explicitly includes a server-side request forgery or local-file access task, follow the room's stated test target and restrictions. Use a controlled lab URL rather than probing public or internal systems that are not in scope.

For each test, record:
- The baseline input and output.
- The changed input and exact response.
- Whether the converter performed an observable action.
- Which component likely interpreted the input.
- Alternative explanations for the result.

A generated PDF containing reflected text is not proof of server-side request forgery or local-file disclosure. The evidence must show that the renderer accessed the resource or executed the relevant operation.

### 4. Root cause and remediation

Escape or sanitize Markdown-to-HTML output using a well-maintained allowlist sanitizer; disable raw HTML when not required; run the renderer in a sandbox with restricted filesystem permissions; block unnecessary network egress; use temporary directories; apply timeouts and file-size limits; and avoid passing user-controlled values into shell commands. Do not rely only on a blacklist of strings.

### 5. Room questions and flags

Map each question to the exact request, PDF behaviour, or server-side result observed in the active room. Record the actual value, not a copied example. Keep sensitive output out of public notes and redact any flag as `flag{hidden}`.
