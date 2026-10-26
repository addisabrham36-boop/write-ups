# Google Dorking

{% embed url="https://tryhackme.com/room/googledorking" %}

An introduction to search engine architecture, web crawling, `robots.txt`, sitemaps, and advanced search operators (Google Dorking) for OSINT and security reconnaissance.

---

## 1. How Search Engines Work

Search engines use automated bots ("Crawlers" or "Spiders") that traverse the web by following links from page to page.
- **Crawling:** The technique used to fetch and discover web pages.
- **Index:** A vast, searchable database storing discovered documents and their locations.
- **Keywords:** The extractable terms and metadata collected to categorize pages for relevance.

---

## 2. Robots.txt

The `robots.txt` file sits at the root of a website (e.g. `ablog.com/robots.txt`) and guides search engine crawlers on what they are allowed or forbidden to index.

Example syntax:
```text
User-agent: Bingbot
Allow: /

User-agent: *
Disallow: /dont-index-me/
Disallow: /*.conf$
```

- Configuration files (`.conf`) and admin areas are often disallowed, inadvertently leaking sensitive directory locations to penetration testers.

---

## 3. Sitemaps

A sitemap is an **XML** formatted document (usually at `/sitemap.xml`) providing a structured **map** of all the content, URLs, and **routes** available on the domain.

---

## 4. Google Dorking Operators

Google Dorking leverages special search operators to filter results with precision:

| Operator | Function | Example |
|---|---|---|
| `site:` | Restrict queries to a specific domain | `site:bbc.co.uk flood defences` |
| `filetype:` | Search for specific file extensions | `filetype:pdf "confidential"` |
| `intitle:` | Search for terms appearing in page title | `intitle:login` |
| `inurl:` | Search for terms appearing in the URL string | `inurl:admin` |
| `cache:` | View Google's cached snapshot of a page | `cache:tryhackme.com` |

---

## 🏁 Questions & Answers

| Task | Question | Answer |
|---|---|---|
| **Task 2** | Collection of resources and their locations created by a crawler | `index` |
| **Task 2** | Technique search engines use to retrieve website information | `crawling` |
| **Task 2** | Type of content gathered from a website | `keywords` |
| **Task 4** | Location of robots.txt on `ablog.com` | `ablog.com/robots.txt` |
| **Task 4** | Standard path where a sitemap is located | `/sitemap.xml` |
| **Task 4** | Directive to only allow Bingbot to index the site | `User-agent: Bingbot` |
| **Task 4** | Directive to prevent crawling of `/dont-index-me/` | `Disallow: /dont-index-me/` |
| **Task 4** | Unix/Linux configuration file extension hidden from crawlers | `.conf` |
| **Task 5** | Typical file structure of a Sitemap | `XML` |
| **Task 5** | Real-life example a sitemap is compared to | `map` |
| **Task 5** | Keyword for the path taken for content on a website | `route` |
| **Task 6** | Format to query `bbc.co.uk` for `flood defences` | `site:bbc.co.uk flood defences` |
| **Task 6** | Search term used to search by file type | `filetype:` |
| **Task 6** | Search term used to look for login pages | `intitle:login` |
