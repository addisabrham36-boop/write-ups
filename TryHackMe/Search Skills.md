# Search Skills

Official room: https://tryhackme.com/room/searchskills

## Room Overview & Information

**Category:** OSINT and security research  
**Difficulty:** Beginner  
**Focus:** Finding technical documentation, researching vulnerabilities, searching internet-facing services, and validating information.

Cybersecurity research often fails because the analyst uses the wrong source, not because the information is impossible to find. The goal of this room is to choose the right search tool for the question, use focused queries, and verify the result before treating it as evidence.

## Phase 1: Build a research question

Before searching, decide what information is missing. Examples:
- What does a tool option do? Use the official manual or documentation.
- Does a vulnerability affect a particular version? Use the vendor advisory and CVE references.
- What services has a public IP exposed? Use an authorised asset inventory or internet-wide search service.
- Is an indicator associated with known malicious activity? Consult reputable threat-intelligence sources and preserve the source and date.

Search terms should include exact product names, versions, error messages, or relevant function names. A broad query such as “Linux security” is less useful than a query that includes the exact command and error.

## Phase 2: Shodan

Shodan indexes internet-facing services observed by its crawlers. It can help an authorised assessor understand what service banners are visible for an asset. It is not a guarantee that the service is still online or vulnerable.

Common Shodan filters include:

| Filter | Purpose | Example |
|---|---|---|
| `country:` | Filter by country code | `country:IE` |
| `port:` | Filter by port | `port:22` |
| `org:` | Filter by organisation | `org:"Example Organisation"` |
| `hostname:` | Filter by hostname | `hostname:example.com` |
| `product:` | Match a product label | `product:Apache` |

Use only the room's supplied target or an asset you are authorised to investigate. A query such as `port:22` is global and broad; add an authorised hostname, organisation, or IP filter where possible.

### Task 2: Shodan (TryScanMe)

The room's Shodan exercise introduces searching for exposed services. Enter the query required by the task in the room's search interface and inspect the result fields. Pay attention to the host, port, product/banner, organisation, and last-observed information when available.

A banner can be missing, stale, or misleading. Treat it as an observation that may guide further verification, not as proof of a confirmed vulnerability.

## Phase 3: Search official technical documentation

For tool usage, prefer:
1. Official documentation or a manual page.
2. Vendor security advisories.
3. The relevant CVE record or trusted vulnerability database.
4. Maintainer discussions or issue trackers for implementation details.
5. Community write-ups as supporting context, not the only source.

On Linux, local manual pages are useful:

```bash
man nmap
man curl
man ssh
```

To search manual-page descriptions:

```bash
apropos network
```

The `man` command opens the local manual for a tool. `apropos` searches manual-page names and short descriptions. Local documentation may be older than the current online documentation, so check version-specific details where accuracy matters.

## Phase 4: Research a vulnerability

A useful vulnerability research workflow is:

1. Identify the exact product and version.
2. Search for a vendor advisory and CVE identifier.
3. Confirm the affected version range and prerequisites.
4. Check whether a patch or workaround exists.
5. Read the technical details to understand the root cause.
6. Verify any proof of concept in an isolated lab.
7. Record the source, date, and version tested.

Do not assume that a search result containing a CVE number applies to every version of the product. Version matching and configuration are essential.

## Phase 5: Validate sources

For every important claim, ask:
- Who published the information?
- Is it the original source or a copied summary?
- Does the date match the question?
- Does it refer to the same product and version?
- Can a second independent source corroborate it?
- Is the information a confirmed fact, an allegation, or an analyst's interpretation?

Search snippets are abbreviated and may be stale. Open the original page and read the relevant section before copying a value into a report.

## Phase 6: Working through the room's questions

For each task:
1. Identify which service or research source the question names.
2. Use the tool supplied by the room or the official source relevant to the question.
3. Apply the requested filter or search term.
4. Inspect the actual result rather than relying on the snippet alone.
5. Copy the exact requested field, keeping the source and date in your notes.
6. If the service is unavailable, follow the room's access instructions instead of substituting a guessed answer.

The room includes examples that depend on current internet data. Search results can change, so a universal answer should not be fabricated when the active task's result is the source of truth.

## Phase 7: Responsible use

Internet-wide search can reveal services that were not intended to be public. Discovery does not grant permission to log in, exploit, or access data. Limit testing to assets within the authorised scope and report accidental exposure through the appropriate channel.

## Key takeaways

Good security research is a repeatable process: define the question, choose the right source, use precise search terms, verify the result, and document uncertainty. Shodan helps investigate observed public services, while official documentation and vendor advisories are better sources for tool behaviour and vulnerability impact.

## Phase 8: Task Evidence Checklist

For every search question, capture the query, source, date, and exact evidence supporting the answer. Verify the original page where possible rather than relying on the search snippet.

| Search need | Query pattern | What to verify |
|---|---|---|
| Search one domain | site:example.com | Hostname and page context |
| Exact phrase | "exact phrase" | Exact wording on the source page |
| Find a document format | filetype:pdf topic | Original file and publication date |
| Find text in a title | intitle:keyword | Page title and actual content |
| Exclude irrelevant term | keyword -excluded | Whether the exclusion changed results as intended |
| Research a product or CVE | Product/version or CVE in quotes | Vendor advisory and affected version |

An old snippet may point to content that has changed or disappeared. When two sources conflict, record the discrepancy. Prefer official advisories and primary sources for technical facts.

## Final reporting principles

Keep claims traceable to a source. Do not copy an entire copyrighted article into notes, and do not republish exposed credentials or private data. Search results are leads, not proof that a live service remains exposed or that an asset is in scope for active testing.
