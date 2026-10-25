# Search Skills: TryHackMe Walkthrough

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

## Expanded search and verification workflow

### 1. Translate the question into search terms

Identify the entity, time range, source type, and exact field requested. Start with a narrow search and add one qualifier at a time. Quoted phrases search for exact wording; a minus sign excludes a term; site restrictions can focus results on an official domain.

### 2. Search operators

Examples:

```text
"exact phrase"
site:example.com keyword
filetype:pdf topic
intitle:report topic
keyword -irrelevant
```

Replace examples with terms relevant to the task. Operators are search aids, not proof that a result is accurate or current.

### 3. Verify the original source

Open the original page or document and check the publication date, author, context, and whether the requested detail actually appears. Search snippets can be truncated, cached, or misleading. For technical questions, prefer official documentation, primary advisories, and vendor references over copied summaries.

### 4. Evaluate source quality

Check who published the information, the evidence supporting it, the date, the intended audience, and whether other reliable sources corroborate it. An opinion post, vendor marketing page, archived page, and official advisory serve different purposes. Use the source type appropriate to the claim.

### 5. Preserve reproducibility

Record the search terms, source URL, publication date, and exact evidence supporting the answer. If the page may change, capture a relevant excerpt in the lab notes where permitted. Do not copy copyrighted articles wholesale or expose credentials found during research.

### 6. Answer the room

For each task, identify the expected entity or value and verify it from the source the exercise expects. Be careful with dates, names, domains, and similar-looking terms. If two sources conflict, explain the conflict instead of choosing the answer that merely looks familiar. Redact any flag as `flag{hidden}`.
