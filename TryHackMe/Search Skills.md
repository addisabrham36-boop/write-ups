# Search Skills: TryHackMe Walkthrough

Room: https://tryhackme.com/room/searchskills

Security work often starts with a question rather than a tool. Search skills help locate documentation, advisories, technical explanations, and evidence efficiently. The goal is not to collect the most links; it is to find reliable information that answers the actual question.

## 1. Define the question

Before searching, identify the exact thing you need to learn: a product version, an error message, a protocol behaviour, a vulnerability advisory, or the meaning of a log field. A precise question helps avoid broad results that are unrelated to the task.

For technical errors, search the exact error text in quotation marks, then add the product or version. For a vulnerability, include the product name, version, CVE identifier if known, and terms such as advisory or mitigation.

## 2. Use search operators

Useful operators include:
- Quotation marks to search for an exact phrase.
- site: to restrict results to a domain.
- filetype: to find a specific document format.
- minus sign to exclude a term.
- OR to search for alternatives where supported.

Examples:
    site:docs.example.com authentication
    "exact error message" product
    site:nvd.nist.gov CVE-YYYY-NNNN
    filetype:pdf incident response guide

These are patterns, not answers. Replace the example domain and placeholder CVE with the actual topic. Search operators vary between engines and may be interpreted differently.

## 3. Evaluate source quality

Prefer primary sources for facts about a product or vulnerability: official documentation, vendor advisories, standards, source code, or a relevant research paper. Independent write-ups can provide helpful context, but verify important claims against primary evidence.

Check the publication date, affected versions, scope, assumptions, and whether the source describes a confirmed observation or a hypothesis. An old guide may refer to a different version or a retired endpoint.

## 4. Verify rather than copy

When a search result suggests a command or answer, understand what it does before running it. Test harmless commands in a local lab, compare results with the official documentation, and record the software version and environment. Never run an unknown script as administrator simply because a forum post recommends it.

For security findings, corroborate a claim with more than one source where possible. A result may be repeated across many sites even though all of them copied the same incorrect original.

## 5. Keep useful research notes

Record the question, search terms, best sources, publication dates, conclusion, and remaining uncertainty. Save the official advisory or documentation URL, not just a screenshot of a search result. This makes later verification easier when the product or threat changes.

## 6. Working through the room

Follow the task's search challenge and use the operator it is teaching. If it asks for information from a particular website, constrain the query to that site and inspect the source result. If it asks about a phrase, search the phrase precisely. Copy the answer from the verified source rather than guessing from the result title.

## What I took from this room

Good search is a repeatable research method: define the question, use precise queries, evaluate source quality, verify the claim, and preserve the source so the answer can be checked later.