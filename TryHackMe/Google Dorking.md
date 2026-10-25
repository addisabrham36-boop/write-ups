# Google Dorking — TryHackMe

Room: https://tryhackme.com/room/googledorking

Google dorking is the practice of using search operators to find information that ordinary searches may miss. It is not a magical hacking button. It is a way to narrow search results using clues about page titles, URLs, file types, and exact phrases.

## Useful operators

Some common operators are:

```text
site:example.com
filetype:pdf
intitle:"index of"
inurl:login
"exact phrase"
```

- `site:` limits results to a domain or site.
- `filetype:` looks for a particular file extension.
- `intitle:` searches page titles.
- `inurl:` looks for text in a URL.
- Quotation marks search for an exact phrase.

Operators can be combined. For example, `site:example.com filetype:pdf` searches for PDFs indexed under that domain. The results depend on what the search engine has crawled and indexed, so an empty result does not prove the information does not exist.

## Why this matters for security

Search engines sometimes index documents, directory listings, test pages, or configuration details that an organisation did not intend to expose. During an authorised assessment, these findings can help identify accidental information disclosure.

Finding something in a search result does not grant permission to access, download, or misuse it. I should stay within the room's scope and avoid collecting personal or sensitive information that is not needed for the task.

## My workflow

I start with a broad domain search, then narrow it using one operator at a time. If I combine too many filters immediately, I may accidentally hide the result I am looking for. I also check whether a result is current and whether the page actually belongs to the organisation, rather than trusting the search snippet alone.

## What I took from this room

Google dorking is a lightweight OSINT technique. The main skill is knowing which operator narrows the search in the right way and interpreting the result carefully. Search engines can expose clues, but they are not complete inventories of a website and the results need verification.