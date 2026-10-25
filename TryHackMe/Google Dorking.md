# Google Dorking: TryHackMe Walkthrough

Room: https://tryhackme.com/room/googledorking

Google dorking is the use of search operators to narrow public search results. Security researchers use it to discover indexed pages, exposed documents, forgotten subdomains, and publicly accessible files. It is passive reconnaissance: the search engine returns information it has already indexed, although opening a result may contact the target site.

## 1. Build a query from operators

Useful operators include:

- site: limits results to a domain or subdomain.
- filetype: limits results to a file format.
- intitle: searches for terms in page titles.
- inurl: searches for terms in URLs.
- quoted phrases search for a specific sequence of words.
- A minus sign excludes a term from results.

Examples:

    site:example.com
    site:example.com filetype:pdf
    site:example.com intitle:"index of"
    site:example.com inurl:login
    "specific phrase" site:example.com

Replace example.com with the domain specified by the room or a domain you are authorised to investigate. Search results are not exhaustive and can be stale, so a missing result does not prove that a resource does not exist.

## 2. Search for public documents

Filetype searches can reveal PDFs, spreadsheets, presentations, or text documents that a site owner did not expect to be easy to find. If a document appears sensitive, do not download, redistribute, or use its contents beyond the authorised scope. Record the result URL and report the exposure responsibly.

An indexed file is not necessarily a vulnerability. It may be intended for public release. The important questions are whether the information is meant to be public, whether it contains secrets or personal data, and whether the organisation has approved its publication.

## 3. Understand directory-listing results

Queries such as a search for pages titled “index of” may surface web servers with directory listing enabled. Directory listings can expose filenames and application structure. They are not automatically exploitable, but they may reveal backups, old archives, configuration files, or source code that should not be public.

Never assume that a search result proves a file is accessible now. Open only resources within the permitted scope and verify the response without attempting to access unrelated private data.

## 4. Subdomain and login discovery

A site-limited query can help identify public pages on a domain, including login portals, staging sites, documentation, and legacy endpoints. Search engines do not provide a complete asset inventory. Organisations should combine passive search with authorised DNS, certificate, and asset-management checks.

Do not use a discovered login page as permission to test credentials. Finding a public endpoint and having permission to attack it are separate things.

## 5. Record findings

For each useful result, record:

1. The query used.
2. The result URL and date.
3. What the page appears to expose.
4. Whether the content is actually public and relevant.
5. The potential impact if the exposure is unintended.
6. The recommended fix, such as removing a file, changing access controls, or requesting de-indexing after the source is secured.

Search engine removal alone does not fix a file that remains publicly accessible. Secure the origin first, then address cached or indexed copies.

## 6. Working through the room

Read the task's objective and construct a query that uses the requested operator. If it asks for a file type, use filetype; if it limits results to a domain, use site; if it asks about a title or URL path, use intitle or inurl. Inspect the results and apply the question's exact criteria rather than guessing an answer from the operator alone.

Search results vary by region, time, and indexing. If the room uses a specific domain or example, use that domain rather than copying the generic examples here.

## 7. Defensive lessons

Organisations should maintain an inventory of public assets, prevent secrets and backups from entering public web roots, disable unnecessary directory listings, and use appropriate authentication and access controls. Search operators can help identify exposure, but they cannot replace configuration review and continuous monitoring.

## What I took from this room

Google dorking is a way to organise publicly indexed information, not a magical way to see every hidden file. The strongest workflow uses precise operators, verifies the finding, respects scope, and reports exposures without accessing data that is not needed.