# Google Dorking

Official room: https://tryhackme.com/room/googledorking

## Room Overview & Information

**Category:** OSINT / web reconnaissance  
**Difficulty:** Beginner  
**Focus:** Search-engine crawlers, indexing, robots.txt, sitemaps, search operators, and finding publicly indexed content.

Google dorking uses advanced search operators to narrow results to a site, file type, exact phrase, or other characteristic. It does not magically bypass access controls: it finds content that the search engine has discovered and indexed. The technique is useful for authorised asset discovery and for checking whether an organisation accidentally exposed documents.

Only investigate assets within the room or an explicitly authorised scope. Do not use search results to access private information or exploit an exposed system.

## Phase 1: Search engines and crawlers

A search engine discovers URLs, fetches publicly reachable pages, extracts information, and indexes selected content so it can be retrieved by queries. Crawlers may discover pages from links, sitemaps, and other known sources. Indexing is not instantaneous and does not include every page.

A page can be reachable but not indexed, indexed but later removed, or indexed with a stale snippet. Search results are therefore evidence of what the search engine knows, not a complete inventory of the current website.

### Task 1: Ye Ol' Search Engine

The task introduces search engines as large indexes of web content. The core idea is that search queries can be refined to make the results more useful. Rather than guessing a URL, an analyst can use search operators to narrow the index.

## Phase 2: Crawlers and site discovery

Crawlers follow links and retrieve pages. They may inspect metadata, headings, links, and page content. The data they collect is processed and stored in a search index.

### Task 2: Let's Learn About Crawlers

When a crawler discovers a page, it can follow links found on that page to discover other pages. This makes a site's internal linking structure important to discoverability. A sitemap can provide a direct list of intended URLs, while robots.txt communicates crawl preferences to compliant crawlers.

Robots.txt is not an access-control mechanism. A disallowed path can still be visited directly if the server does not enforce authentication and authorisation. Publishing a sensitive path in robots.txt may even draw attention to it.

## Phase 3: Robots.txt and sitemaps

A robots.txt file is normally located at the root of a website:

```text
https://example.com/robots.txt
```

It contains rules for compliant crawlers, such as which paths they should avoid crawling. It does not protect the contents of those paths.

A sitemap is commonly an XML file that lists URLs intended for discovery. Its typical structure is XML, often at a path such as `/sitemap.xml`, though a site may use another location and can list a sitemap in robots.txt.

### Task 4: Beepboop: Robots.txt

The task asks about the purpose of robots.txt and sitemaps. The key distinctions are:
- robots.txt expresses crawler instructions.
- a sitemap lists URLs to help crawlers discover content.
- neither feature replaces server-side access control.

If the task asks for a sitemap's typical structure, inspect the example shown by the room. A conventional sitemap is an XML document with a `urlset` root and `url` entries, often containing `loc` elements.

If the task asks what a sitemap is comparable to, think of a map or directory of routes to website content. If it asks for the keyword describing a path through a site, use the exact terminology presented in the room's example and verify it against the task text.

## Phase 4: Search operators

Operators refine the query. Examples:

```text
site:example.com
site:example.com "incident response"
filetype:pdf site:example.com
intitle:"index of" site:example.com
"exact phrase"
site:example.com -www
```

- `site:` restricts results to a domain or site.
- Quotation marks search for an exact phrase.
- `filetype:` narrows results by a file extension or type.
- `intitle:` searches page titles.
- A leading minus excludes a term.

These examples use example.com and should be adapted only to an authorised target. Search-engine syntax and behaviour can change, and not every operator is supported identically across engines.

### Task 6: What is Google Dorking?

Google dorking combines ordinary terms with operators to make targeted searches. For example:

```text
site:example.com filetype:pdf
```

This searches the index for PDF results associated with example.com. It does not guarantee that every PDF on the site will appear.

For a room question, copy the exact domain or phrase the task provides, apply the operator described by the task, and inspect the result title and URL. Do not assume the first result is always correct; the room may ask for a particular file, page, or value.

## Phase 5: Common security uses

Search operators can help authorised defenders find:
- Public documents that should not have been published.
- Old copies of pages or files.
- Login or administrative pages that are publicly discoverable.
- Publicly exposed error messages or directory listings.
- File types that may contain metadata.

Discovery is not proof of a vulnerability. A login page may be intentionally public, a document may be intended for publication, and a search snippet may be stale. Verify the actual exposure and report it responsibly.

## Phase 6: Practical workflow for the room

1. Read the task and identify the domain or sample site it names.
2. Open the specified search or SEO tool.
3. Use the exact query required by the task.
4. Inspect the returned URL, title, and relevant snippet.
5. If the task concerns robots.txt or a sitemap, open the file and inspect its contents.
6. Copy only the value the question asks for.
7. If a result is absent, verify the query syntax and use the room's alternative search tool as instructed.

Some questions use interactive or external services. Their output may change, and access can depend on the network environment. Use the live room's data rather than inventing a result.

## Phase 7: Defensive controls

Do not rely on obscurity. Require authentication for sensitive pages, enforce authorisation on the server, disable directory listings where unnecessary, remove exposed files, and avoid placing secrets in publicly accessible documents. Use robots.txt only to guide compliant crawlers, not to protect resources.

## Key takeaways

Search engines are indexes, not security boundaries. Crawlers discover content, robots.txt gives crawl instructions, sitemaps aid discovery, and search operators narrow results. Dorking is useful for finding exposure, but every result must be validated against the current website and the authorised scope.

## Phase 8: Search task verification

For each task, translate the question into an operator and a source, then verify the original result.

| Task focus | Useful approach | Evidence to preserve |
|---|---|---|
| Find pages for a domain | site:example.com | The result URL and title |
| Find exact wording | Quoted phrase | Original page containing the phrase |
| Find exposed documents | filetype:pdf site:example.com | Document title and URL |
| Find paths named admin | site:example.com inurl:admin | The indexed URL, not a guessed route |
| Find directory listing pages | intitle:"index of" plus an in-scope term | Result title and accessible directory contents |
| Check indexing instructions | /robots.txt and sitemap.xml | The exact directives or URL list |

Search results can be stale, and a Google snippet may omit important context. Open the source where permitted and verify the requested value on the page. Don't turn a public search discovery into active testing without explicit authorization.

## Final defensive checklist

Do not publish secrets or private files on a public web root. Use authentication and server-side authorization, disable directory listings when unnecessary, remove stale files, and review search-index results after a sensitive exposure has been fixed. Robots.txt and sitemaps support discovery; they do not protect the underlying content.
