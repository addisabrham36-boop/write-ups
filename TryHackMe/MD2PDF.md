# MD2PDF: TryHackMe Walkthrough

Room: https://tryhackme.com/room/md2pdf

MD2PDF is a web application challenge built around converting Markdown content into a PDF. Document conversion services are a useful security case study because they accept user-controlled content and pass it through parsers, templates, renderers, or other helper tools. The exact vulnerability must be established from the active target.

## 1. Enumerate the target

Start the room and record the assigned IP. Run a service scan:

    nmap -sC -sV -oN md2pdf-nmap.txt TARGET_IP

Open the web service and inspect the converter interface, form fields, upload options, and response behaviour. Check the page source and any public paths referenced by the application. If directory discovery is required, use a wordlist available in the lab and inspect results manually.

## 2. Understand the conversion pipeline

A converter may accept Markdown text or a file, parse it, render HTML, and then generate a PDF. Each step has different risks. Markdown may allow embedded links or HTML, the renderer may load local or remote resources, and a helper process may run with filesystem permissions.

Map the input to the output: what can the user control, which component processes it, and what does the response reveal? Do not assume the service has command injection, server-side template injection, or server-side request forgery until the evidence supports it.

## 3. Test inputs carefully

Use the lab's intended test cases and change one property at a time. Compare the output PDF, error message, response time, and any observable network or file behaviour. A failed conversion may indicate invalid Markdown rather than a security weakness.

If the task reveals that local files or remote resources are being accessed, confirm the behaviour with the smallest safe proof. Do not read unrelated files or probe external hosts beyond the scope.

## 4. Review file and process permissions

If authorised access to the host is part of the room, identify the user under which the converter runs and what directories it can access. A conversion service should not run with root privileges or have access to secrets unrelated to its task.

Temporary files should be created in a controlled directory and cleaned up safely. User-provided filenames should not be allowed to escape the intended storage path. Helper processes should use fixed executables and structured arguments rather than concatenated shell commands.

## 5. Document the vulnerability

Record the affected input, the observed behaviour, why the behaviour crosses a security boundary, and the impact. Distinguish a confirmed finding from a hypothesis. If the room provides a flag, obtain it from the live instance and redact it as flag{hidden} in the public version.

## 6. Remediation

- Run the converter with least privilege.
- Isolate document rendering from sensitive files and internal services.
- Disable unnecessary external resource loading.
- Validate file type and size, and use safe temporary-file handling.
- Avoid shell invocation and unsafe template evaluation.
- Patch the renderer and its dependencies.
- Add tests for malicious Markdown, path traversal, and unexpected resource requests.

## What I took from this room

A file-conversion feature is still an application attack surface. The security review needs to follow untrusted content through parsing, rendering, helper processes, and file access, then restrict each component to only what it needs.